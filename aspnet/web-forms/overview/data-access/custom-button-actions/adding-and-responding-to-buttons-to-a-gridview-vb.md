---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: "加入和按鈕的 GridView (VB) 來回應 |Microsoft 文件"
author: rick-anderson
description: "在此教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們會 bui..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: ba66c867df93a9e1a0bb7897052ace7300b672af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>加入和回應的 GridView (VB) 的按鈕
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe)或[下載 PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> 在此教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們會建置具有可讓使用者逐頁瀏覽供應商 FormView 的介面。


## <a name="introduction"></a>簡介

雖然許多報表案例牽涉到唯讀存取報表資料，它不常見的報表包含了可執行動作的 s 根據顯示的資料。 這通常涉及加入按鈕、 LinkButton 或 ImageButton Web 控制項與每一筆記錄顯示在報表中，按一下時，會導致回傳，並叫用一些伺服器端程式碼。 編輯和刪除記錄的記錄為基礎的資料是最常見的範例。 實際上，如同我們可了解開頭[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，編輯和刪除是常見的 GridView、 DetailsView 和 FormView 控制項可支援這類功能，而不會需要撰寫一行程式碼。

此外編輯和刪除按鈕、 GridView、 DetailsView 和 FormView 控制項也可以包含按鈕、 LinkButtons 或 ImageButtons，按一下時，執行一些自訂的伺服器端邏輯。 在此教學課程中我們將探討如何將自訂按鈕，新增至範本和 GridView 或 DetailsView 控制項的欄位。 特別是，我們會建置具有可讓使用者逐頁瀏覽供應商 FormView 的介面。 在 FormView 給定的供應商，顯示按鈕 Web 控制項，如果按下，會在標記其相關聯的產品的所有停用以及供應商的相關資訊。 此外，GridView 列出所選取的供應商，提供提高價格與折扣價格的按鈕，如果按下，提高或降低產品 s，其中包含每個資料列的那些產品`UnitPrice`（請參閱圖 1） 的 10%。


[![FormView 和 GridView 包含執行自訂動作的按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**圖 1**： 兩個 FormView 和 GridView 包含按鈕，執行自訂動作 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>步驟 1： 加入按鈕的教學課程 Web 網頁

我們會審視如何加入自訂按鈕之前，可讓 s 先花點時間在我們的網站專案，我們需要在此教學課程中建立 ASP.NET 網頁。 加入新的資料夾，名為啟動`CustomButtons`。 接下來，將下列兩個 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `CustomButtons.aspx`


![如需自訂的按鈕相關教學課程中加入 ASP.NET 網頁](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁的自訂按鈕相關教學課程


如同在其他資料夾，`Default.aspx`中`CustomButtons`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


最後，將頁面加入做為項目至`Web.sitemap`檔案。 具體來說，加入下列標記之後的分頁和排序`<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入和刪除教學課程的項目。


![站台地圖現在包含之項目的自訂按鈕教學課程](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**圖 4**： 網站導覽現在包含之項目的自訂按鈕教學課程


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>步驟 2： 加入列出供應商 FormView

可讓開始與本教學課程將會列出供應商在 FormView 的 s。 如簡介中所討論，此 FormView 可讓使用者逐頁檢視供應商，顯示在 GridView 中供應商所提供的產品。 此外，此 FormView 會包含一個按鈕，按一下時，會在標記的所有供應商的產品已停止。 我們會將自訂的按鈕加入到 FormView 考量自己之前，可讓第一次在 FormView 剛建立，使其顯示供應商的資訊。

先開啟`CustomButtons.aspx`頁面`CustomButtons`資料夾。 FormView 加入頁面，藉由拖曳從工具箱拖曳至設計工具和設定其`ID`屬性`Suppliers`。 來自 FormView s 智慧標籤，選擇 建立新的 ObjectDataSource 名為`SuppliersDataSource`。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**圖 5**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


設定此新 ObjectDataSource，它會查詢從`SuppliersBLL`類別的`GetSuppliers()`方法 （請參閱圖 6）。 因為此 FormView 不提供用於更新的供應商資訊，請選取 （無） 選項從下拉式清單中 更新 索引標籤的介面。


[![資料來源設定為使用 SuppliersBLL 類別的 GetSuppliers() 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**圖 6**: 資料來源設定為使用`SuppliersBLL`類別 s`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


設定後 ObjectDataSource，Visual Studio 會產生`InsertItemTemplate`， `EditItemTemplate`，和`ItemTemplate`在 FormView 的。 移除`InsertItemTemplate`和`EditItemTemplate`和修改`ItemTemplate`，使其顯示只供應商的公司名稱和電話號碼。 最後，請在 FormView 的分頁支援藉由檢查其智慧標籤啟用分頁 核取方塊 (或藉由設定其`AllowPaging`屬性`True`)。 在這些變更之後頁面 s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

圖 7 顯示 [CustomButtons.aspx] 頁面上透過瀏覽器檢視時。


[![在 FormView 列出 CompanyName 和電話，目前選取的供應商的欄位](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**圖 7**: FormView 列出`CompanyName`和`Phone`商之目前選取的欄位 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>步驟 3： 加入列出選取的供應商的產品的 GridView

我們將停止所有的產品 按鈕加入至 FormView 的範本之前，可讓第一次加入下方會列出所選取的供應商提供的產品在 FormView 的 GridView 的 s。 若要達成此目的，將 GridView 加入頁面中，設定其`ID`屬性`SuppliersProducts`，並新增名為新 ObjectDataSource `SuppliersProductsDataSource`。


[![建立名為 SuppliersProductsDataSource 新 ObjectDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**圖 8**： 建立新的 ObjectDataSource 具名`SuppliersProductsDataSource`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


設定此 ObjectDataSource 使用 ProductsBLL 類別的`GetProductsBySupplierID(supplierID)`方法 （請參閱圖 9）。 雖然這個 GridView 會允許調整產品的價格，贏了 t 會使用內建編輯或刪除從 GridView 的功能。 因此，我們可以設定為 （無） 下拉式清單，ObjectDataSource s UPDATE、 INSERT 和 DELETE 的索引標籤。


[![資料來源設定為使用 ProductsBLL 類別的 GetProductsBySupplierID(supplierID) 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**圖 9**: 資料來源設定為使用`ProductsBLL`類別 s`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


因為`GetProductsBySupplierID(supplierID)`方法會接受輸入的參數、 ObjectDataSource 精靈會提示輸入我們此參數值的來源。 傳入`SupplierID`來自 FormView 值、 控制項和 ControlID 下拉式清單，以設定參數來源下拉式清單`Suppliers`（步驟 2 中建立的程式設計識別碼）。


[![表示讓 supplierID 參數應該都來自供應商 FormView 控制項](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**圖 10**： 表示 *`supplierID`* 參數應該來自於`Suppliers`FormView 控制項 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


完成 ObjectDataSource 精靈之後，GridView 會包含 BoundField 或 CheckBoxField 每個產品的資料欄位。 可讓 s 修剪這顯示只`ProductName`和`UnitPrice`連同 BoundFields `Discontinued` CheckBoxField; 此外，可讓 s 格式`UnitPrice`BoundField 使其文字會格式化為貨幣。 您的 GridView 和`SuppliersProductsDataSource`ObjectDataSource s 宣告式標記看起來應該類似下列標記：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

此時教學課程中我們會顯示主要/詳細資料報表，讓使用者從頂端 FormView 中選取 供應商，並檢視底部 GridView 透過該供應商所提供的產品。 圖 11 顯示此頁面的螢幕擷取畫面來自 FormView 選取東京 Traders 供應商時。


[![選取 供應商的產品會顯示在 GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**圖 11**: GridView 中會顯示選取的供應商的產品 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>步驟 4： 建立 DAL 和 BLL 方法，以停止所有產品的供應商

我們在 FormView 中加入按鈕之前，按一下時，會停止所有的供應商的產品中，我們需要將方法加入至 DAL 和 BLL 執行此動作。 特別是，這個方法將名為`DiscontinueAllProductsForSupplier(supplierID)`。 按一下 FormView 的按鈕時，我們將會叫用商務邏輯層中的，這個方法傳遞中選取的供應商 s `SupplierID`; BLL 再將呼叫向下對應的資料存取層方法，即會發出`UPDATE`陳述式停止指定的供應商的產品資料庫。

因為我們已完成在我們先前的教學課程中，我們會使用由下而上的方法，從建立 DAL 方法，然後 BLL 方法，以及最後 ASP.NET 網頁中實作的功能。 開啟`Northwind.xsd`中具類型資料集`App_Code/DAL`資料夾並將新方法加入`ProductsTableAdapter`(以滑鼠右鍵按一下`ProductsTableAdapter`，然後選擇 加入查詢)。 這樣會顯示 「 TableAdapter 查詢組態精靈 」，我們逐步解說的程序加入新的方法。 指出我們 DAL 的方法會使用特定 SQL 陳述式開始。


[![建立使用特定 SQL 陳述式的 DAL 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**圖 12**： 建立 DAL 方法使用特定 SQL 陳述式 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


接下來，精靈會提示我們來建立查詢的類型。 因為`DiscontinueAllProductsForSupplier(supplierID)`方法將需要更新`Products`資料庫資料表時，設定`Discontinued`欄位設為 1，指定所提供的所有產品 *`supplierID`* ，我們要建立查詢，以更新資料。


[![選擇 更新查詢類型](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**圖 13**： 選擇更新的查詢類型 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


下一個畫面中，精靈會提供現有 TableAdapter s`UPDATE`陳述式，就會更新每個欄位中定義`Products`DataTable。 此查詢的文字取代為下列陳述式：


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

輸入這項查詢，並按一下 下一步，最後一個畫面中，精靈會要求新的方法的名稱後使用`DiscontinueAllProductsForSupplier`。 完成精靈，依序按一下 [完成] 按鈕。 傳回要在 DataSet 設計工具時您應該會看到新的方法中`ProductsTableAdapter`名為`DiscontinueAllProductsForSupplier(@SupplierID)`。


[![新的 DAL 方法 DiscontinueAllProductsForSupplier 名稱](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**圖 14**： 將新的 DAL 方法`DiscontinueAllProductsForSupplier`([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


與`DiscontinueAllProductsForSupplier(supplierID)`資料存取層中建立的方法下, 一步是建立`DiscontinueAllProductsForSupplier(supplierID)`商務邏輯層中的方法。 若要完成這項作業，開啟`ProductsBLL`類別檔案，並加入下列：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

這個方法只會呼叫向下`DiscontinueAllProductsForSupplier(supplierID)`中 DAL 提供傳遞方法 *`supplierID`* 參數值。 如果沒有任何只允許在某些情況下停止 s 產品的供應商的商務規則，這些規則應該在這裡，在實作 BLL。

> [!NOTE]
> 不同於`UpdateProduct`中多載`ProductsBLL`類別`DiscontinueAllProductsForSupplier(supplierID)`方法簽章不包含`DataObjectMethodAttribute`屬性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 如此即無需`DiscontinueAllProductsForSupplier(supplierID)`從 ObjectDataSource s 設定資料來源精靈的下拉式清單，在 [更新] 索引標籤中的方法。我已省略此屬性，因為我們將會呼叫`DiscontinueAllProductsForSupplier(supplierID)`直接從我們的 ASP.NET 網頁中的事件處理常式方法。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>步驟 5： 加入中止所有產品按鈕，以在 FormView

與`DiscontinueAllProductsForSupplier(supplierID)`BLL 和 DAL 中的方法完成時，加入要在選取的供應商是將按鈕 Web 控制項加入 FormView s 停止所有產品的能力的最終步驟`ItemTemplate`。 可讓 s 新增這類按鈕下方的按鈕文字時，中斷所有產品的供應商的電話號碼和`ID`屬性值為`DiscontinueAllProductsForSupplier`。 您可以編輯樣板中的連結 FormView s 智慧標籤，即可加入透過設計工具的這個按鈕 Web 控制項 （請參閱圖 15），或直接透過宣告式語法。


[![新增中止 FormView 的 ItemTemplate 的所有產品按鈕 Web 控制項](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**圖 15**： 將停止所有產品按鈕 Web 控制項加入 FormView s `ItemTemplate` ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


當按鈕按下使用者造訪的網頁回傳展示和 FormView s [ `ItemCommand`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formview.itemcommand.aspx)引發。 若要執行自訂程式碼以回應在按下此按鈕，我們可以建立此事件的事件處理常式。 了解，不過，`ItemCommand`事件引發時*任何*FormView 內按下按鈕、 LinkButton 或 ImageButton Web 控制項。 這表示當使用者從一頁移到另 FormView 中`ItemCommand`事件引發，則當使用者按一下 [新增]，[編輯]，或在支援插入、 更新或刪除 FormView 中刪除相同的動作。

因為`ItemCommand`引發不論按一下按鈕時，事件處理常式我們需要一個方法來判斷是否按下 [停止所有的產品] 按鈕，或如果是某些其他按鈕。 若要達成此目的，我們可以設定 Button Web 控制項的`CommandName`屬性設為識別值。 當按一下按鈕時，這`CommandName`值會傳遞至`ItemCommand`事件處理常式，讓我們能夠判斷是否 button 已按下 [停止所有的產品] 按鈕。 設定停止所有產品 按鈕的`CommandName`DiscontinueProducts 的屬性。

最後，可讓使用用戶端確認對話方塊中，以確保使用者確實想要停止選取的供應商的產品 s。 如我們所見中[新增用戶端確認時刪除](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md)教學課程，即可達成這位元的 JavaScript。 特別是，將按鈕 Web 控制項的 OnClientClick 屬性設定為`return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

進行這些變更之後，來自 FormView s 的宣告式語法看起來應該如下所示：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

接下來，建立事件處理常式 FormView s`ItemCommand`事件。 此事件處理常式中，我們需要先判斷是否已按下 [停止所有的產品] 按鈕。 因此，我們要建立的執行個體如果`ProductsBLL`類別，並叫用其`DiscontinueAllProductsForSupplier(supplierID)`方法，傳入`SupplierID`選取在 formview:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

請注意， `SupplierID` FormView 中目前選取的供應商可以存取使用 FormView s [ `SelectedValue`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)。 `SelectedValue`屬性會傳回第一個資料機碼顯示在 FormView 中記錄的值。 在 FormView s [ `DataKeyNames`屬性](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.formview.datakeynames.aspx)，指出的資料欄位的資料索引鍵的值取自，自動設定為`SupplierID`繫結至 FormView 後的 ObjectDataSource 時的 Visual studio在步驟 2。

與`ItemCommand`事件處理常式建立，請花一點時間來測試頁面。 瀏覽至 Cooperativa de Quesos ' Las Cabras' 供應商 (它 s 我 FormView 中的第五個供應商)。 此供應商提供這兩項產品，Queso Cabrales 和 Queso Manchego La Pastora，兩者都是*不*已停止。

假設 Cooperativa de Quesos ' Las Cabras' 已離開公司，因此其產品會停止。 按一下 [中斷所有產品] 按鈕。 這會顯示用戶端確認對話方塊 （請參閱圖 16）。


[![Cooperativa de Quesos Las Cabras 提供兩個使用中的產品](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**圖 16**: Cooperativa de Quesos Las Cabras 提供兩個使用中的產品 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


如果您按一下 確定，在用戶端確認 對話方塊中的，表單送出將會繼續，導致回傳所在 FormView 的`ItemCommand`事件就會引發。 我們建立的事件處理常式會再執行，請叫用`DiscontinueAllProductsForSupplier(supplierID)`方法，並停用 Queso Cabrales 和 Queso Manchego La Pastora 產品。

如果您已停用 GridView 的檢視狀態，GridView 會被重新繫結至基礎資料存放區在每次回傳，並因此將立即更新以反映，這兩項產品現已停用 （請參閱圖 17）。 不過，您無法停用在 GridView 的檢視狀態，如果您必須以手動方式重新繫結至 GridView 進行這項變更後。 若要達成此目的，只對進行呼叫 GridView s`DataBind()`方法叫用後，立即`DiscontinueAllProductsForSupplier(supplierID)`方法。


[![按一下 中斷所有產品按鈕後，供應商的產品是據此更新](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**圖 17**： 按一下 中斷所有產品按鈕後，供應商的產品是據此更新 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>步驟 6： 建立 UpdateProduct 多載中調整產品的價格的商務邏輯層

像以停止所有產品中的按鈕在 FormView 中，若要加入按鈕，以提高並減少在 GridView 的產品的價格我們要先加入適當的資料存取層和商務邏輯層方法。 因為我們已更新單一產品中的資料列 DAL 的方法，我們可以建立的新多載來提供這類功能`UpdateProduct`BLL 中的方法。

我們過去`UpdateProduct`多載在產品欄位的某種組合做為輸入的純量值中取得，然後更新欄位的指定產品。 這個多載，我們將會稍微不同，從這項標準，改為傳入產品 s`ProductID`以及用來調整百分比`UnitPrice`(而不是在新的傳遞，調整`UnitPrice`本身)。 此方法會簡化我們需要在 ASP.NET 網頁程式碼後置類別中撰寫的程式碼，因為我們不需要擔心判斷目前的產品 s t `UnitPrice`。

`UpdateProduct`多載，如本教學課程中所示：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

這個多載所擷取到 DAL s 指定的產品資訊`GetProductByProductID(productID)`方法。 然後它會檢查以查看是否產品 s`UnitPrice`指派資料庫`NULL`值。 如果是，價格會保持不變。 如果，不過，有非`NULL``UnitPrice`值，此方法會更新產品 s`UnitPrice`所指定的百分比 (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>步驟 7： 加入增加和減少按鈕 GridView

GridView （和 DetailsView） 是兩組成欄位的集合。 除了 BoundFields、 CheckBoxFields 和 TemplateFields，ASP.NET 會包含 ButtonField，其中，正如其名，會轉譯成與按鈕、 LinkButton 或 ImageButton 每個資料列的資料行。 FormView 中，按一下類似*任何*內 GridView 分頁按鈕、 編輯或刪除按鈕，排序按鈕，等等 按鈕會導致回傳，並引發 GridView s [ `RowCommand`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)。

具有 ButtonField`CommandName`屬性，將指定的值指派給每個按鈕`CommandName`屬性。 在 FormView 中，例如`CommandName`值由`RowCommand`來判斷哪一個按鈕已按下的事件處理常式。

O d e s 加入兩個新 ButtonFields 至 GridView，其中一個按鈕文字價格 + 10%，而另一個文字價格-10%。 若要新增這些 ButtonFields，按一下 編輯資料行連結從 GridView s 智慧標籤上選取 ButtonField 欄位型別從左上角的清單，按一下 新增 按鈕。


![在 GridView 中加入兩個 ButtonFields](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**圖 18**: GridView 中加入兩個 ButtonFields


移動兩個 ButtonFields，使其顯示為前兩個 GridView 欄位。 接下來，設定`Text`屬性 Price + 10%到這些兩個 ButtonFields 和價格-10%和`CommandName`屬性，以便 IncreasePrice 以及 DecreasePrice，分別。 根據預設，ButtonField 會轉譯成 LinkButtons 按鈕的其資料行。 這可以變更，不過，透過 ButtonField s [ `ButtonType`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)。 將已轉譯為規則的按鈕; 這些兩個 ButtonFields s因此，設定`ButtonType`屬性`Button`。 圖 19 顯示欄位 對話方塊之後已經進行這些變更。下列的是 GridView s 宣告式標記。


![設定 ButtonFields 文字、 CommandName 和 ButtonType 屬性](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**圖 19**： 設定 ButtonFields `Text`， `CommandName`，和`ButtonType`屬性


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

與建立這些 ButtonFields，最後一個步驟是建立事件處理常式 GridView s`RowCommand`事件。 如果因為所以引發此事件處理常式的其中一個價格 + 10%或價格-10%按鈕已按下，必須判斷`ProductID`其按鈕已按下並接著叫用的資料列`ProductsBLL`類別的`UpdateProduct`方法，傳入適當`UnitPrice`連同調整百分比`ProductID`。 下列程式碼會執行這些工作：

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

若要判斷`ProductID`資料列價格 + 10%或價格-10%按鈕已按下，我們需要參閱 GridView 的`DataKeys`集合。 這個集合會保存欄位中指定的值`DataKeyNames`每個 GridView 資料列的屬性。 GridView s 自`DataKeyNames`屬性設定為 ProductID 由 Visual Studio 時繫結至 GridView 的 ObjectDataSource`DataKeys(rowIndex).Value`提供`ProductID`指定*rowIndex*。

在自動傳遞 ButtonField *rowIndex*的 button 已按下透過資料列的`e.CommandArgument`參數。 因此，若要判斷`ProductID`資料列的價格 + 10%或價格-10%按鈕已按下，我們使用： `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`。

做為停止所有產品 按鈕，如果您已停用 GridView 的檢視狀態，GridView 是正在重新繫結至基礎資料存放區在每次回傳，且因此將立即更新以反映從按一下，就會發生的價格變更其中一個按鈕。 不過，您無法停用在 GridView 的檢視狀態，如果您必須以手動方式重新繫結至 GridView 進行這項變更後。 若要達成此目的，只對進行呼叫 GridView s`DataBind()`方法叫用後，立即`UpdateProduct`方法。

圖 20 顯示網頁時檢視祖母 Kelly Homestead 所提供的產品。 圖 21 顯示的結果之後價格 + 10%已按一下按鈕兩次的祖母的 Boysenberry 散佈和價格-10%按鈕一次的 Northwoods Cranberry 秘技。


[![在 GridView 包含價格 + 10%和價格-10%按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**圖 20**: GridView 包括價格 + 10%和價格-10%按鈕 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![第一個和第三個產品的價格都已經透過價格 + 10%和價格-10%按鈕](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**圖 21**: 價格的第一個和第三個產品都已經透過價格 + 10%和價格-10%按鈕 ([按一下以檢視完整大小的影像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> 按鈕、 LinkButtons 或加入至其 TemplateFields ImageButtons，也可以有 GridView （和 DetailsView）。 因為使用 BoundField 按一下時，這些按鈕會誘使回傳，引發 GridView 的`RowCommand`事件。 當加入按鈕中 TemplateField，不過，按鈕的`CommandArgument`不會自動設定為資料列的索引，所以使用 ButtonFields 時。 如果您需要決定內所點選按鈕的資料列索引`RowCommand`事件處理常式，您必須手動設定按鈕的`CommandArgument`TemplateField，類似的程式碼中宣告式語法中的屬性：  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>總結

所有的 GridView、 DetailsView 和 FormView 控制項可以包含按鈕、 LinkButtons 或 ImageButtons。 這類的按鈕，按一下時，會導致回傳，並引發`ItemCommand`在 FormView 和 DetailsView 控制項的事件和`RowCommand`GridView 中的事件。 這些資料的 Web 控制項有內建功能，以處理一般命令相關的動作，例如刪除或編輯資料錄。 不過，我們可以也使用按鈕，在按下時，以執行自己的自訂程式碼回應。

若要達成此目的，我們需要建立事件處理常式`ItemCommand`或`RowCommand`事件。 此事件處理常式中，我們先檢查傳入`CommandName`值，以判斷哪一個按鈕已按下然後再採取適當的自訂動作。 在本教學課程中，我們看到如何使用按鈕和 ButtonFields 停止指定的供應商的所有產品，或以增加或減少 10%的特定產品的價格。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一步](adding-and-responding-to-buttons-to-a-gridview-cs.md)
