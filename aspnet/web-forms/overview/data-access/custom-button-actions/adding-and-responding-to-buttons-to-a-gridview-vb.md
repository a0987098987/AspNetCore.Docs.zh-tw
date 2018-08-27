---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: 新增與回應 GridView (VB) 的按鈕 |Microsoft Docs
author: rick-anderson
description: 在本教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們將建置...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0834d43f95bd19fffb603dcde640714bd779fd80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831153"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>新增與回應 GridView (VB) 的按鈕
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe)或[下載 PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> 在本教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們將建置具有可讓使用者逐頁瀏覽供應商 FormView 的介面。


## <a name="introduction"></a>簡介

雖然許多報表案例牽涉到唯讀模式存取報表資料，它顯示的資料為基礎的報表包含執行動作的能力不常見的 s。 通常這牽涉到將按鈕、 LinkButton 或 ImageButton Web 控制項與顯示在報表中每個記錄，按一下時，會導致回傳，並叫用一些伺服器端程式碼。 編輯和刪除記錄的記錄為基礎的資料是最常見的範例。 事實上，如我們所見開頭[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，編輯和刪除它是陳腔 GridView、 DetailsView 和 FormView 控制項可支援這類功能，而不用需要撰寫任何一行程式碼。

除了編輯和刪除按鈕、 GridView、 DetailsView 和 FormView 控制項也可以包含按鈕、 Linkbutton 或 ImageButtons，按一下時，執行一些自訂的伺服器端邏輯。 在本教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們將建置具有可讓使用者逐頁瀏覽供應商 FormView 的介面。 對於給定的供應商、 FormView 會顯示的按鈕 Web 控制項，如果按下，會標記其相關聯的產品的所有停用以及供應商的相關資訊。 此外，GridView 會列出所選取的供應商，增加的價格與折扣價格的按鈕，如果按下，提高或降低產品 s，其中包含每個資料列提供這些產品`UnitPrice`（請參閱 圖 1） 的 10%。


[![FormView 和 GridView 包含執行自訂動作的按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**圖 1**： 這兩個 FormView 和 GridView 包含按鈕，執行自訂動作 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>步驟 1： 加入按鈕教學課程的 Web 網頁

我們看看如何新增自訂按鈕之前，讓先花點時間在網站專案中，我們需要在此教學課程中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`CustomButtons`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的下列兩個 ASP.NET 頁面`Site.master`主版頁面：

- `Default.aspx`
- `CustomButtons.aspx`


![加入 ASP.NET 網頁的自訂按鈕相關教學課程](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁的自訂按鈕相關教學課程


在其他資料夾，例如`Default.aspx`在`CustomButtons`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


最後，將頁面新增項目，以作為`Web.sitemap`檔案。 具體來說，在分頁和排序之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入及刪除教學課程的項目。


![網站導覽現在包含項目自訂按鈕教學課程](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**圖 4**： 網站地圖現在包含項目自訂按鈕教學課程


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>步驟 2： 加入列出的供應商 FormView

可讓開始進行本教學課程將會列出供應商 FormView 的 s。 如簡介中所述，此 FormView 可讓使用者逐頁檢視供應商，顯示 GridView 中供應商所提供的產品。 此外，此 FormView 會包含一個按鈕，按一下時，會在標記的所有供應商的產品已停止。 我們考慮自行加入 FormView 的自訂按鈕之前，讓第一次 FormView 只建立，使其顯示供應商資訊。

首先開啟`CustomButtons.aspx`頁面中`CustomButtons`資料夾。 加入頁面中的 FormView，藉由拖曳從工具箱拖曳至設計工具和設定其`ID`屬性設`Suppliers`。 從 FormView s 智慧標籤，選擇建立新的 ObjectDataSource 名為`SuppliersDataSource`。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**圖 5**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


它會從查詢，請設定這個新的 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法 （請參閱 圖 6）。 由於此 FormView 不更新供應商資訊，選取 （無） 選項從下拉式清單中，在 更新 索引標籤中提供的介面。


[![資料來源設定為使用 SuppliersBLL 類別的 GetSuppliers() 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**圖 6**： 設定要使用的資料來源`SuppliersBLL`類別 s`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Visual Studio 會產生之後設定 ObjectDataSource `InsertItemTemplate`， `EditItemTemplate`，和`ItemTemplate`FormView 的。 移除`InsertItemTemplate`並`EditItemTemplate`，並修改`ItemTemplate`，讓它顯示只是供應商的公司名稱和電話號碼。 最後，開啟 FormView 的分頁支援，藉由檢查它的智慧標籤啟用分頁 核取方塊 (或藉由設定其`AllowPaging`屬性設`True`)。 在這些變更之後頁面 s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

[圖 7] 顯示 CustomButtons.aspx 頁面上，當透過瀏覽器檢視。


[![FormView 列出 [CompanyName] 和從目前選取的供應商的電話欄位](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**圖 7**: FormView 列出`CompanyName`並`Phone`從目前選取的供應商的欄位 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>步驟 3： 新增的 GridView 會列出所選的供應商產品

我們將停止所有的產品 按鈕新增至 FormView 的範本之前，讓第一次新增下方會列出所選取的供應商提供的產品 FormView GridView 的 s。 若要達成此目的，將 GridView 加入頁面中，設定其`ID`屬性，以`SuppliersProducts`，並新增名為新 ObjectDataSource `SuppliersProductsDataSource`。


[![建立名為 SuppliersProductsDataSource 新 ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**圖 8**： 建立新的 ObjectDataSource 具名`SuppliersProductsDataSource`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


設定要使用 ProductsBLL 類別的 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法 （請參閱 圖 9）。 此 GridView 會讓產品 s 價格會調整，並同時贏了 t 會使用內建編輯或刪除從 GridView 的功能。 因此，我們可以設定為 （無） 下拉式清單，ObjectDataSource s 的更新、 插入和刪除的索引標籤。


[![資料來源設定為使用 ProductsBLL 類別的 GetProductsBySupplierID(supplierID) 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**圖 9**： 設定要使用的資料來源`ProductsBLL`類別 s`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


因為`GetProductsBySupplierID(supplierID)`方法會接受輸入的參數、 ObjectDataSource 精靈會提示我們輸入這個參數值的來源。 傳入`SupplierID`FormView 值、 控制項和 ControlID 下拉式清單中的，若要設定參數來源下拉式清單`Suppliers`（在步驟 2 中建立的 FormView 識別碼）。


[![表示讓 supplierID 參數應該都來自供應商 FormView 控制項](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**圖 10**： 表示 *`supplierID`* 參數應該來自於`Suppliers`FormView 控制項 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


完成 ObjectDataSource 精靈之後，GridView 會包含 BoundField 或 CheckBoxField 每個產品的資料欄位。 可讓 s 精簡這顯示只`ProductName`並`UnitPrice`BoundFields 連同`Discontinued`CheckBoxField; 此外，可讓 s 格式`UnitPrice`BoundField 使其文字格式化為貨幣。 您的 GridView 和`SuppliersProductsDataSource`ObjectDataSource s 宣告式標記看起來應該類似下列標記：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

此時在教學課程中會顯示主版/詳細資料報表，讓使用者從頂端 FormView 挑選供應商，並檢視底部 GridView 透過該供應商所提供的產品。 [圖 11] 顯示此頁面的螢幕擷取畫面從 FormView 選取東京 Traders 供應商時。


[![選取的供應商產品會顯示在 GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**圖 11**: 選取的供應商的產品都會顯示在 GridView ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>步驟 4： 建立 DAL 和 BLL 方法，以停止所有產品的供應商

我們可在 FormView 上加入按鈕之前，按一下時，會中止所有供應商 s 產品中，我們必須先將方法加入的 DAL 」 和 「 不會執行此動作的 BLL。 特別是，這個方法將會命名為`DiscontinueAllProductsForSupplier(supplierID)`。 按一下 FormView 的按鈕時，我們將會叫用這個方法在商務邏輯層中，傳遞中選取的供應商 s `SupplierID`; BLL 會接著呼叫向下對應的資料存取層方法會發出`UPDATE`陳述式停止指定的供應商的產品資料庫。

我們已完成之後，我們先前的教學課程中，我們將使用由下往上的方法，開始建立 DAL 方法，然後 BLL 方法，和最後 ASP.NET 網頁中實作的功能。 開啟`Northwind.xsd`型別中的資料集`App_Code/DAL`資料夾，並新增新的方法來`ProductsTableAdapter`(以滑鼠右鍵按一下`ProductsTableAdapter`，然後選擇 新增查詢)。 這樣會帶出 TableAdapter 查詢組態精靈，它將帶領我們新增新方法的程序。 指出我們的 DAL 方法會使用特定 SQL 陳述式開始。


[![建立使用特定 SQL 陳述式的 DAL 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**圖 12**： 建立 DAL 方法使用特定 SQL 陳述式 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


接下來，精靈會提示我們輸入有關何種查詢來建立。 由於`DiscontinueAllProductsForSupplier(supplierID)`方法將需要更新`Products`資料庫資料表中，設定`Discontinued`欄位設為 1，指定所提供的所有產品 *`supplierID`* ，我們需要建立可更新資料的查詢。


[![選擇更新查詢類型](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**圖 13**： 選擇更新查詢類型 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


精靈的下一個畫面會提供現有 TableAdapter s`UPDATE`陳述式，這會更新每個欄位中定義`Products`DataTable。 此查詢的文字取代為下列陳述式：


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

輸入這項查詢，並按一下 [下一步]，精靈的最後一個畫面會要求新的方法的名稱之後使用`DiscontinueAllProductsForSupplier`。 按一下 [完成] 按鈕，以完成精靈。 返回 DataSet 設計工具中，您應該看到中的新方法`ProductsTableAdapter`名為`DiscontinueAllProductsForSupplier(@SupplierID)`。


[![命名新的 DAL 方法 DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**圖 14**： 將新的 DAL 方法命名`DiscontinueAllProductsForSupplier`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


具有`DiscontinueAllProductsForSupplier(supplierID)`在資料存取層中建立的方法下, 一步是建立`DiscontinueAllProductsForSupplier(supplierID)`商業邏輯層中的方法。 若要這麼做，開啟`ProductsBLL`類別檔案，並新增下列：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

這個方法只會呼叫下`DiscontinueAllProductsForSupplier(supplierID)`方法中傳遞所提供的 DAL *`supplierID`* 參數值。 如果有任何僅允許產品在某些情況下停止供應商的商務規則，這些規則應該在這裡，在實作 BLL。

> [!NOTE]
> 不同於`UpdateProduct`中的多載`ProductsBLL`類別`DiscontinueAllProductsForSupplier(supplierID)`方法簽章不包含`DataObjectMethodAttribute`屬性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 這會防止`DiscontinueAllProductsForSupplier(supplierID)`從 ObjectDataSource s 設定資料來源精靈的下拉式清單中 [更新] 索引標籤的方法。我已省略此屬性，因為我們將會呼叫`DiscontinueAllProductsForSupplier(supplierID)`直接從我們的 ASP.NET 網頁中的事件處理常式的方法。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>步驟 5： 新增停止所有的產品按鈕，以 FormView

具有`DiscontinueAllProductsForSupplier(supplierID)`BLL 和 DAL 中的方法完成時，加入選取的供應商是 Button Web 控制項加入 FormView s 停止所有產品的能力的最後一個步驟`ItemTemplate`。 讓 s 中加入這類按鈕下方的按鈕文字時，停止所有產品的供應商的電話號碼和`ID`屬性值為`DiscontinueAllProductsForSupplier`。 您可以將此按鈕 Web 控制項透過設計工具中 FormView s 智慧標籤的 編輯範本 連結上即可 （請參閱 圖 15），或直接透過宣告式語法。


[![將停止所有產品按鈕 Web 控制項，以 FormView 的 ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**圖 15**: 停止所有產品 Web 將按鈕控制項都加入到 FormView `ItemTemplate` ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


當使用者瀏覽 頁面上，回傳是兩邊彼此乾瞪眼和 FormView s 按一下按鈕[`ItemCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx)引發。 若要執行自訂程式碼以回應所按下此按鈕，我們可以建立此事件的事件處理常式。 了解，不過`ItemCommand`就會引發事件時*任何*FormView 內按下按鈕、 LinkButton 或 ImageButton Web 控制項。 這表示當使用者從一頁移到另一個在 FormView，`ItemCommand`事件引發; 相同的動作，當使用者按一下 [新增]，[編輯]，或在支援插入、 更新或刪除的 FormView 中刪除。

因為`ItemCommand`引發不論按一下按鈕時，在事件處理常式我們需要方法來判斷是否按下 [停止所有的產品] 按鈕，或如果是其他的按鈕。 若要這麼做，我們可以設定按鈕的 Web 控制項的`CommandName`某些識別值的屬性。 當按一下按鈕時，這`CommandName`值會傳遞至`ItemCommand`事件處理常式，讓我們可以判斷是否停止所有的產品 按鈕已按下按鈕。 設定停止所有的產品按鈕 s `CommandName` DiscontinueProducts 的屬性。

最後，讓使用用戶端確認對話方塊中，以確保使用者真正想要停止選取的供應商的產品的 s。 如我們在中所見[正在刪除時新增用戶端確認](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md)教學課程中，這可以使用一些 JavaScript 來完成。 特別是，將 Button Web 控制項的 OnClientClick 屬性設定為 `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

進行這些變更之後，FormView s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

接下來，建立事件處理常式 FormView s`ItemCommand`事件。 這個事件處理常式中，我們需要先判斷是否已按下 [停止所有的產品] 按鈕。 如果我們想要建立的執行個體的的話`ProductsBLL`類別，並叫用其`DiscontinueAllProductsForSupplier(supplierID)`方法並傳入`SupplierID`選取 FormView 的：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

請注意， `SupplierID` FormView 中目前選取的供應商可以存取使用 FormView s [ `SelectedValue`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)。 `SelectedValue`屬性會傳回第一個資料機碼顯示在 FormView 中之資料錄的值。 FormView s [ `DataKeyNames`屬性](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)，表示的資料欄位的資料索引鍵值取自，自動設定為`SupplierID`時繫結至 FormView 後的 ObjectDataSource 的 Visual studio在步驟 2。

使用`ItemCommand`事件處理常式建立，請花一點時間測試頁面。 瀏覽至 Cooperativa de Quesos ' Las Cabras' 供應商 (它 s 我 FormView 中的第五個供應商)。 此供應商提供兩項產品，Queso Cabrales 和 Queso Manchego La Pastora，這兩者都是*不*停用。

假設 Cooperativa de Quesos ' Las Cabras' 已離開公司，因此其產品會停止。 按一下 [停止所有產品] 按鈕。 這會顯示用戶端確認對話方塊 （請參閱 圖 16）。


[![Cooperativa de Quesos Las Cabras 提供兩個使用中的產品](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**圖 16**: Cooperativa de Quesos Las Cabras 提供兩個使用中的產品 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


如果您按一下 [確定]，在用戶端確認對話方塊中，表單送出將會繼續，在其中造成回傳 FormView 的`ItemCommand`事件就會引發。 我們建立的事件處理常式接著會執行，請叫用`DiscontinueAllProductsForSupplier(supplierID)`方法，並停用的 Queso Cabrales 」 和 「 Queso Manchego La Pastora 產品。

如果您已停用的 GridView 的檢視狀態，GridView 會被重新繫結至基礎資料存放區在每次回傳中，並因此將立即更新以反映，這兩項產品現已停用 （請參閱 圖 17）。 如果，不過，您不停用檢視狀態，在 gridview 裡，您必須以手動方式將重新繫結至 GridView 資料之後進行這項變更。 若要這麼做，只要呼叫到 GridView`DataBind()`方法叫用後，立即`DiscontinueAllProductsForSupplier(supplierID)`方法。


[![按一下 停止所有產品按鈕後，供應商的產品是據以更新](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**圖 17**： 按一下 停止所有產品按鈕後，供應商的產品是據以更新 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>步驟 6： 建立 UpdateProduct 多載中調整產品的價格的商務邏輯層

像是停止所有產品中的按鈕 FormView，若要加入按鈕的增加和減少 GridView 裡的產品的價格我們都需要先將新增適當的資料存取層和商務邏輯層方法。 由於我們已經有更新單一產品中的資料列的 DAL 的方法，我們可以建立的新多載來提供這類功能`UpdateProduct`到 BLL 中的方法。

我們過去`UpdateProduct`多載已在產品欄位的一些組合，做為純量輸入的值，然後更新的欄位指定的產品。 這個多載而言，我們會稍有不同這項標準，改為傳入產品 s`ProductID`以及用來調整百分比`UnitPrice`(而不是在新的傳遞，調整`UnitPrice`本身)。 這個方法會簡化我們需要在 ASP.NET 網頁程式碼後置類別中撰寫的程式碼，因為我們不需要擔心判斷目前的產品 s t `UnitPrice`。

`UpdateProduct`多載，如本教學課程中所示：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

這個多載擷取透過 DAL s 指定的產品的相關資訊`GetProductByProductID(productID)`方法。 然後它會檢查以查看是否產品 s`UnitPrice`指派資料庫`NULL`值。 如果是，價格會保持不變。 如果，不過，有非`NULL``UnitPrice`值，這個方法會更新產品 s`UnitPrice`所指定的百分比 (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>步驟 7： 新增至 GridView 的增加和減少按鈕

GridView （和 DetailsView） 是兩組成欄位的集合。 除了 BoundFields、 CheckBoxFields 和 TemplateFields，ASP.NET 會包含 ButtonField，其中，正如其名，會轉譯成與按鈕、 LinkButton 或 ImageButton 的資料行，每個資料列。 類似於 FormView，按一下*任何*內 GridView 分頁按鈕、 編輯或刪除按鈕，排序按鈕和等等的按鈕會導致回傳，並引發 GridView s [ `RowCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)。

具有 ButtonField`CommandName`屬性，將指定的值指派給每個其按鈕`CommandName`屬性。 使用 FormView`CommandName`會使用值`RowCommand`事件處理常式，來判斷所按的按鈕。

可讓 s 新增兩個新 ButtonFields 至 GridView，一個使用的按鈕文字價格 + 10%，而另一個文字價格-10%。 若要新增這些 ButtonFields，按一下 [編輯資料行] 連結，從 GridView s 智慧標籤選取 ButtonField 欄位型別從清單中的左上方，按一下 [新增] 按鈕。


![新增兩個 ButtonFields 至 GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**圖 18**： 新增兩個 ButtonFields 至 GridView


移動兩個 ButtonFields，使它們顯示為前兩個 GridView 欄位。 接下來，設定`Text`屬性，這些兩個 ButtonFields 價格 + 10%的價格介於-10%，`CommandName`屬性給 IncreasePrice 和 DecreasePrice，分別。 根據預設，ButtonField 會將其資料行的按鈕呈現為 Linkbutton。 這可以變更，不過，透過 ButtonField s [ `ButtonType`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)。 讓已轉譯為一般的推播按鈕; 這些兩個 ButtonFields s因此，設定`ButtonType`屬性設`Button`。 圖 19 顯示欄位 對話方塊中進行這些變更; 之後接著是 GridView s 宣告式標記。


![設定 ButtonFields 文字、 CommandName、 和 ButtonType 屬性](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**圖 19**： 設定 ButtonFields `Text`， `CommandName`，和`ButtonType`屬性


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

建立這些 ButtonFields，最後一個步驟是建立事件處理常式 GridView s`RowCommand`事件。 這個事件處理常式中引發，因為如果任一個價格 + 10%或價格-10%按鈕已按下，需求，來決定`ProductID`其按鈕已按下，然後叫用之資料列`ProductsBLL`類別的`UpdateProduct`方法並傳入適當`UnitPrice`連同百分比調整`ProductID`。 下列程式碼會執行下列工作：

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

若要判斷`ProductID`資料列的價格 + 10%或價格-10% 按鈕已按下，我們需要將 GridView s，請參閱`DataKeys`集合。 這個集合會保存欄位中指定的值`DataKeyNames`每個 GridView 資料列的屬性。 因為 GridView s`DataKeyNames`屬性設定為 ProductID 由 Visual Studio 時繫結至 GridView 的 ObjectDataSource`DataKeys(rowIndex).Value`提供`ProductID`指定*rowIndex*。

在自動傳遞 ButtonField *rowIndex*其按鈕已按下透過資料列的`e.CommandArgument`參數。 因此，若要判斷`ProductID`資料列的價格 + 10%或價格-10% 按鈕已按下，我們使用： `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`。

做為 [停止所有產品] 按鈕，如果您已停用 GridView 的檢視狀態，GridView 是正在重新繫結至基礎資料存放區在每次回傳中，與因此將立即更新以反映從按一下，就會發生的價格變更其中一個按鈕。 如果，不過，您不停用檢視狀態，在 gridview 裡，您必須以手動方式將重新繫結至 GridView 資料之後進行這項變更。 若要這麼做，只要呼叫到 GridView`DataBind()`方法叫用後，立即`UpdateProduct`方法。

[圖 20] 顯示頁面檢視祖母 Kelly Homestead 所提供的產品時。 圖 21 顯示的結果之後價格 + 10%按鈕被按兩次的祖母的 Boysenberry 散佈和價格-10%按鈕一次的 Northwoods Cranberry 醬。


[![GridView 包含價格 + 10%和價格-10%按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**圖 20**: GridView 包含價格 + 10%和價格-10%按鈕 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![已更新的第一個和第三個產品的價格透過價格 + 10%和價格-10%按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**圖 21**: 價格的第一個和第三個產品都已經透過價格 + 10%和價格-10%按鈕 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> 按鈕、 Linkbutton 或新增至其 TemplateFields ImageButtons，也可以有 GridView （和 DetailsView）。 因為 BoundField 與這些按鈕，按一下時，會引發回傳，引發 GridView 的`RowCommand`事件。 當新增按鈕為 TemplateField，不過，按鈕的`CommandArgument`不會自動設定資料列的索引，即使用 ButtonFields 時。 如果您需要決定內按下按鈕的資料列索引`RowCommand`事件處理常式，您必須手動設定 [s] 按鈕`CommandArgument`TemplateField，類似的程式碼中宣告式語法中的屬性：  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>總結

所有的 GridView、 DetailsView 和 FormView 控制項可以包含按鈕、 Linkbutton 或 ImageButtons。 這類的按鈕，按一下時，會造成回傳並引發`ItemCommand`FormView 和 DetailsView 控制項中的事件和`RowCommand`GridView 內的事件。 這些資料 Web 控制項有內建的功能，以處理一般與命令相關的動作，例如刪除或編輯記錄。 不過，我們也可以在使用按鈕，按下時，回應與執行自己的自訂程式碼。

若要達成此目的，我們需要建立的事件處理常式`ItemCommand`或`RowCommand`事件。 這個事件處理常式中，我們先檢查傳入`CommandName`判斷所按的按鈕，然後再採取適當的自訂動作的值。 在本教學課程中，我們看到如何使用按鈕和 ButtonFields 停止指定的供應商的所有產品，或以增加或減少 10%的特定產品的價格。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](adding-and-responding-to-buttons-to-a-gridview-cs.md)
