---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: 從 GridView 的頁尾 (VB) 插入新的記錄 |Microsoft Docs
author: rick-anderson
description: 雖然 GridView 控制項不會插入新記錄的資料提供內建支援，本教學課程會示範如何擴充以包含 GridView...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c661190125e818d3abaf54f50a0067d0944a956
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833484"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>從 GridView 的頁尾 (VB) 插入新的記錄
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe)或[下載 PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> 雖然 GridView 控制項不會插入新記錄的資料提供內建支援，本教學課程會示範如何增強 GridView，以包含插入的介面。


## <a name="introduction"></a>簡介

中所述[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程，GridView、 DetailsView 和 FormView Web 控制項每個包含內建的資料修改功能。 宣告式資料來源控制項搭配使用時，這三個 Web 控制項可以快速且輕鬆地設定來修改資料集和案例，而不需要撰寫任何一行程式碼中。 不幸的是，只有 DetailsView 和 FormView 控制項所提供的內建插入、 編輯和刪除功能。 GridView 只會提供編輯和刪除的支援。 不過，與一些折線油漬，我們可以增強 GridView，以包含插入的介面。

將插入的功能新增至 GridView，我們會負責決定將加入新的記錄、 建立插入的介面，以及撰寫程式碼來插入新的記錄項目。 在本教學課程，我們將探討將插入的介面新增至 GridView 的頁尾資料列 （請參閱 圖 1）。 每個資料行的頁尾資料格包含適當的資料集合使用者介面項目 （產品 s 名稱的文字方塊中，供應商，依此類推 dropdownlist 進行）。 我們也需要資料行的新增按鈕，按下時，會造成回傳，並插入新記錄到`Products`資料表使用的頁尾資料列中提供的值。


[![頁尾資料列中加入新的產品提供的介面](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**圖 1**： 頁尾資料列加入新的產品提供的介面 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>步驟 1: GridView 中顯示產品資訊

我們考慮自行建立 GridView 的頁尾中的插入介面之前，讓 s 著重 GridView 加入列出資料庫中的產品頁面第一次。 首先開啟`InsertThroughFooter.aspx`頁面中`EnhancedGridView`資料夾，然後從 [工具箱] 拖曳至設計工具，設定 GridView 的拖曳的 GridView`ID`屬性設`Products`。 接下來，使用 GridView s 智慧標籤繫結至名為新 ObjectDataSource `ProductsDataSource`。


[![建立名為 ProductsDataSource 新 ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`ProductsDataSource`([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProducts()`方法來擷取產品資訊。 本教學課程中，讓 s 焦點，嚴格上加入插入的功能，不需擔心編輯和刪除。 因此，請確定在 [插入] 索引標籤中的下拉式清單設為`AddProduct()`而更新和刪除索引標籤中的下拉式清單會設定為 （無）。


[![將 AddProduct 方法對應至 ObjectDataSource s insert （） 方法](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**圖 3**： 地圖`AddProduct`方法的 ObjectDataSource s`Insert()`方法 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![設定為 （無） 更新和刪除索引標籤的下拉式清單](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**圖 4**： 將更新和刪除索引標籤下拉式清單會列出為 （無） ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


完成 ObjectDataSource s 設定資料來源精靈之後，Visual Studio 會自動加入欄位至 GridView 的每個對應的資料欄位。 現在，將所有的 Visual Studio 所加入的欄位。 稍後在本教學課程，我們會再回來移除某些欄位的值不需要新增新的記錄時指定。

由於有接近 80 產品在資料庫中，使用者必須捲動到頁面底部的 web，才能加入新的記錄。 因此，可讓啟用分頁功能，是為了讓插入介面，更透明化且可存取的 s。 若要開啟分頁，只要檢查 GridView s 智慧標籤啟用分頁核取方塊。

此時，GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![產品的所有資料欄位會都顯示在分頁的 GridView](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**圖 5**： 在分頁的 GridView 會顯示所有的產品資料欄位 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>步驟 2： 新增頁尾資料列

加上其標頭和資料列，GridView 會包含頁尾資料列。 頁首和頁尾資料列會顯示依據的 GridView 的值[ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx)並[ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx)屬性。 若要顯示頁尾資料列，請設定`ShowFooter`屬性設`True`。 圖 6 所示，設定`ShowFooter`屬性設`True`頁尾資料列加入方格中。


[![若要顯示頁尾資料列，請將 ShowFooter 設定為 True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**圖 6**： 若要顯示頁尾資料列中，設定`ShowFooter`要`True`([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


請注意，頁尾資料列深紅色的背景色彩。 這是因為我們建立並套用至所有頁面 DataWebControls 佈景主題年代[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程。 具體而言，`GridView.skin`檔案會設定`FooterStyle`屬性這類，這會使用`FooterStyle`CSS 類別。 `FooterStyle`類別定義於`Styles.css`，如下所示：


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> 我們已在先前的教學課程中使用 GridView 的頁尾資料列中加以探索。 如有需要請參閱上一步[GridView 的頁尾顯示摘要資訊](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)重新整理程式的教學課程。


在設定後`ShowFooter`屬性設`True`，花一點時間瀏覽器中檢視輸出。 目前的頁尾資料列不 t 所包含的任何文字或 Web 控制項。 在步驟 3 中我們將修改的每個欄位 GridView 的頁尾，使其包含適當的插入介面。


[![空白的頁尾資料列會顯示上面分頁介面控制項](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**圖 7**: 空白的頁尾資料列會顯示上面分頁介面控制項 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>步驟 3： 自訂頁尾資料列

回到[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教學課程中我們了解如何大幅自訂顯示的特定的 GridView 資料行，使用 TemplateFields （相對於 BoundFields 或 CheckBoxFields）; 在[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)我們探討了以自訂編輯介面 GridView 中的使用 TemplateFields。 為 TemplateField 由數個範本，定義混合使用標記、 Web 控制項和資料繫結語法所組成的重新叫用用於某些類型的資料列。 `ItemTemplate`，例如，指定範本用於唯讀的資料列，雖然`EditItemTemplate`定義範本，可編輯的資料列。

連同`ItemTemplate`並`EditItemTemplate`，也包含 TemplateField`FooterTemplate`指定頁尾資料列的內容。 因此，我們可以在其中新增 Web 控制項所需的每個插入到介面的欄位 s `FooterTemplate`。 若要開始，請將所有的 GridView 中的欄位轉換成 TemplateFields。 做法是藉由按一下 GridView s 智慧標籤，在左下角，選取每個欄位，並按一下此欄位轉換成 TemplateField 連結中的 [編輯資料行] 連結。


![每個欄位轉換為 TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**圖 8**： 每個欄位轉換為 TemplateField


按一下 轉換為 TemplateField 此欄位會開啟對等的 TemplateField 到目前的欄位類型。 例如，每個 BoundField 會取代使用 TemplateField `ItemTemplate` ，其中包含顯示相對應的資料欄位的標籤和`EditItemTemplate`文字方塊中顯示資料欄位。 `ProductName` BoundField 已轉換成下列 TemplateField 標記：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

同樣地， `Discontinued` CheckBoxField 已轉換成 TemplateField 其`ItemTemplate`並`EditItemTemplate`包含核取方塊 Web 控制項 (與`ItemTemplate`s 停用的核取方塊)。 唯讀`ProductID`BoundField 已轉換成與標籤控制項中的 TemplateField`ItemTemplate`和`EditItemTemplate`。 簡單地說，轉換現有的 GridView TemplateField 欄位是功能的快速輕易地切換至 更多可自訂的 TemplateField，而不會遺失任何現有的欄位。

因為 GridView 我們處理不 t 支援編輯、 放心地移除`EditItemTemplate`只從每個 TemplateField，離開`ItemTemplate`。 完成之後，GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

現在，每個 GridView 欄位轉換成 TemplateField，我們可以輸入適當的插入介面到每個欄位的`FooterTemplate`。 某些欄位不會插入介面 (`ProductID`，比方說); 其他人會以 Web 控制項可用來收集新的產品的資訊而有所不同。

若要建立的編輯介面，請從 GridView s 智慧標籤中選擇 [編輯範本] 連結。 然後，從下拉式清單中，選取適當的欄位的`FooterTemplate`拖曳適當的控制項從 [工具箱] 拖曳至設計工具。


[![將適當的插入介面新增至每個欄位的 FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**圖 9**： 將適當的插入介面新增至每個欄位 s `FooterTemplate` ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


下列項目符號清單列舉 GridView 欄位，指定要加入的插入介面：

- `ProductID` None。
- `ProductName` 加入文字方塊，並設定其`ID`至`NewProductName`。 新增 RequiredFieldValidator 控制項，以確保使用者輸入的值，讓新的產品的名稱。
- `SupplierID` None。
- `CategoryID` None。
- `QuantityPerUnit` 新增文字方塊中，設定其`ID`至`NewQuantityPerUnit`。
- `UnitPrice` 加入名為 TextBox `NewUnitPrice` CompareValidator，以確保輸入的值是貨幣值大於或等於零。
- `UnitsInStock` 使用文字方塊其`ID`設為`NewUnitsInStock`。 包含 CompareValidator，以確保輸入的值是整數值大於或等於零。
- `UnitsOnOrder` 使用文字方塊其`ID`設為`NewUnitsOnOrder`。 包含 CompareValidator，以確保輸入的值是整數值大於或等於零。
- `ReorderLevel` 使用文字方塊其`ID`設為`NewReorderLevel`。 包含 CompareValidator，以確保輸入的值是整數值大於或等於零。
- `Discontinued` 將新增核取方塊，設定其`ID`至`NewDiscontinued`。
- `CategoryName` 新增 dropdownlist 進行並設定其`ID`至`NewCategoryID`。 繫結至名為新 ObjectDataSource`CategoriesDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategories()`方法。 有 DropDownList s `ListItem` s 顯示器`CategoryName`資料欄位中，使用`CategoryID`做為其值的資料欄位。
- `SupplierName` 新增 dropdownlist 進行並設定其`ID`至`NewSupplierID`。 繫結至名為新 ObjectDataSource`SuppliersDataSource`並將它設定為使用`SuppliersBLL`類別的`GetSuppliers()`方法。 有 DropDownList s `ListItem` s 顯示器`CompanyName`資料欄位中，使用`SupplierID`做為其值的資料欄位。

針對每個驗證控制項中，清除`ForeColor`屬性，讓`FooterStyle`CSS 類別 s 白色的前景色彩會代替紅色的預設值。 也使用`ErrorMessage`屬性的詳細說明，但設定`Text`設為星號的屬性。 若要避免驗證控制項的文字導致插入的介面，以包裝為兩行，將`FooterStyle`s`Wrap`屬性設定為 false，針對每個`FooterTemplate`的驗證控制項。 最後，新增 ValidationSummary 控制項下方的 GridView 和設定其`ShowMessageBox`屬性，以`True`及其`ShowSummary`屬性設`False`。

當新增新的產品，我們需要提供`CategoryID`和`SupplierID`。 這項資訊透過頁尾資料格中的 dropdownlist 進行擷取`CategoryName`和`SupplierName`欄位。 我選擇使用這些欄位相對於`CategoryID`和`SupplierID`TemplateFields 方格 s 中的資料列的使用者因為是可能比較有興趣查看的類別和供應商名稱，而不是其識別碼值。 因為`CategoryID`並`SupplierID`值現在正在擷取中`CategoryName`並`SupplierName`欄位 s 插入介面，我們可以移除`CategoryID`和`SupplierID`從 GridView 的 TemplateFields。

同樣地，`ProductID`未使用時加入新的產品，因此`ProductID`TemplateField 可以一併移除。 不過，讓 s 保留`ProductID`方格中的欄位。 除了文字方塊、 dropdownlist 進行中，核取方塊和組成插入介面的驗證控制項，我們還需要新增按鈕，按一下時，會執行邏輯，以便將新的產品加入至資料庫。 在步驟 4 中，我們會包含插入的介面中的 [新增] 按鈕`ProductID`TemplateField 的`FooterTemplate`。

歡迎改善各種 GridView 欄位的外觀。 比方說，您可能想要格式化`UnitPrice`貨幣值靠右對齊`UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`欄位，以及更新`HeaderText`TemplateFields 的值。

建立插入介面中的許多之後`FooterTemplate`s，移除`SupplierID`，和`CategoryID`TemplateFields，並改善透過格式和對齊 TemplateFields，您的宣告式 GridView s 方格的美術設計標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

當透過瀏覽器檢視，GridView 的頁尾資料列現在包含已完成插入介面 （請參閱 圖 10）。 此時，插入介面不包含使用者指示該她 s 新產品的輸入資料，並想要將新記錄插入資料庫的方法。 此外，我們還可解決頁尾中的輸入資料將如何轉譯中的新記錄的 ve`Products`資料庫。 我們將探討如何包含 [新增] 按鈕插入的介面，以及如何在執行程式碼的步驟 4 中的回傳時它 s 按下。 步驟 5 會示範如何插入新記錄使用頁尾中的資料。


[![GridView 的頁尾提供的介面加入新記錄](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**圖 10**: GridView 的頁尾提供的介面加入新記錄 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>步驟 4： 包括插入介面中的 [新增] 按鈕

我們必須包含 [新增] 按鈕某處插入的介面中因為目前插入介面的頁尾資料列 s 缺少使用者指出他們已完成輸入新的產品的資訊的方法。 這可放在其中一個現有`FooterTemplate`或我們無法將新的資料行加入方格中針對此目的。 本教學課程中，讓 s 放置在 [新增] 按鈕`ProductID`TemplateField 的`FooterTemplate`。

從設計工具中，請按一下 GridView s 智慧標籤中的 [編輯範本] 連結，然後選擇`ProductID`欄位的`FooterTemplate`從下拉式清單。 將按鈕 Web 控制項 （或 LinkButton 或 ImageButton，如果您想） 新增到範本中，將其識別碼設定為`AddProduct`、 其`CommandName`至插入、 並將其`Text`新增，如 圖 11 所示的屬性。


[![置於 ProductID TemplateField 的 FooterTemplate 中的 [新增] 按鈕](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**圖 11**： 將放在 [新增] 按鈕`ProductID`TemplateField s `FooterTemplate` ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


一旦您已包含 [新增] 按鈕，測試的瀏覽器頁面。 請注意，當按一下含無效的資料插入介面中的 新增 按鈕，回傳簡單地說 circuited ValidationSummary 控制項表示無效的資料 （請參閱 圖 12）。 輸入適當的資料，按一下 [新增] 按鈕，將會導致回傳。 沒有記錄會加入至資料庫，不過。 我們必須撰寫的實際執行插入的程式碼。


[![加入按鈕的回傳是簡短 Circuited 中是否有無效的資料插入介面](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**圖 12**: 加入按鈕的回傳是簡短 Circuited 中是否有無效的資料插入介面 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> 插入介面中的驗證控制項不被指派給驗證群組中。 這可正常運作，只要插入介面是唯一的驗證頁面上的控制項集合。 不過，還有其他驗證控制項 （例如格線 s 編輯介面中的驗證控制項） 頁面上，如果驗證控制項中插入介面，並新增按鈕的`ValidationGroup`屬性應該指派相同的值 so 為至將這些控制項與特定的驗證群組產生關聯。 請參閱[剖析 ASP.NET 2.0 中的驗證控制項](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)如需有關驗證群組中分割的驗證控制項和網頁上的按鈕。


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>步驟 5： 插入新記錄到`Products`資料表

當使用內建的編輯功能的 GridView，GridView 會自動處理的所有工作所需執行更新。 特別是，按一下 [更新] 按鈕時它會複製輸入從編輯介面中之 ObjectDataSource 的參數值`UpdateParameters`集合和一時興起，藉由叫用的 ObjectDataSource s update 關閉`Update()`方法。 由於 GridView 不提供插入這類內建功能，我們必須實作程式碼呼叫 s 的 ObjectDataSource`Insert()`方法，並複製的值插入介面為 ObjectDataSource 的`InsertParameters`集合.

已按下 [新增] 按鈕之後，應該執行此插入邏輯。 中所述[新增與回應 GridView 的按鈕](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md)教學課程中，每當按鈕、 LinkButton 或在 GridView 的 ImageButton 按一下時，在 GridView 的`RowCommand`在回傳時引發的事件。 是否] 按鈕，LinkButton、 ImageButton 已新增或明確例如頁尾資料列中的 [新增] 按鈕，或它是否已自動加入的 GridView，就會引發這個事件 (例如選取啟用排序時，每個資料行頂端的 Linkbutton 或Linkbutton 中選取 [啟用分頁時的分頁介面)。

因此，若要回應使用者按一下 [新增] 按鈕，我們需要建立事件處理常式 GridView s`RowCommand`事件。 因為每當引發這個事件*任何*按一下 LinkButton 或在 gridview 裡的 ImageButton 按鈕時，它很重要，我們只進行插入邏輯如果 s`CommandName`屬性傳遞至事件處理常式對應`CommandName`的 [新增] 按鈕 （插入） 的值。 此外，我們也只應該繼續如果驗證控制項報告有效的資料。 若要做到這一點，建立 事件處理常式`RowCommand`為下列程式碼的事件：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> 您可能想知道為什麼事件處理常式困擾檢查`Page.IsValid`屬性。 畢竟，如果無效的資料插入介面中提供，會隱藏獲利的 t 回傳嗎？ 這項假設是正確的只要使用者並未停用 JavaScript，或已採取步驟，避免用戶端驗證邏輯。 簡單地說，應該永遠不會依賴完全在用戶端驗證;有效的伺服器端檢查應該一律執行之前使用的資料。


我們在步驟 1 中建立`ProductsDataSource`ObjectDataSource，其`Insert()`方法會對應至`ProductsBLL`類別的`AddProduct`方法。 要插入至新的資料錄`Products`資料表中，我們只叫用的 ObjectDataSource s`Insert()`方法：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

既然`Insert()`叫用方法，只剩下就是將插入的介面中的值複製到參數傳遞給`ProductsBLL`類別的`AddProduct`方法。 如我們所見年代[檢查與插入、 更新和刪除事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教學課程，即可透過 ObjectDataSource 的`Inserting`事件。 在 `Inserting`我們需要以程式設計方式參考從控制項的事件`Products`GridView 的頁尾資料列，並指派其值`e.InputParameters`集合。 當使用者遺漏值，例如離開`ReorderLevel`我們要指定應插入至資料庫值的文字方塊中空白`NULL`。 由於`AddProducts`方法會接受 null 的型別可為 null 的資料庫欄位，只是使用可為 null 的型別和將其值設定為`Nothing`萬一使用者輸入省略的位置。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

具有`Inserting`事件處理常式中完成，新的記錄可以新增至`Products`透過 GridView 的頁尾資料列的資料庫資料表。 請繼續並再次嘗試新增數個新的產品。

## <a name="enhancing-and-customizing-the-add-operation"></a>增強，以及自訂新增作業

目前，按一下 [新增] 按鈕就會將新記錄加入資料庫資料表，但不提供任何類型的視覺化回饋已成功新增記錄。 在理想情況下，一個 Label Web 控制項或用戶端警示方塊會通知之使用者的成功完成其插入。 我不要更動此練習的讀取器。

本教學課程使用 GridView 列出的產品，不適用於任何排序，也無法讓使用者排序資料。 因此，因為它們是在其主索引鍵欄位的資料庫中，會依據排序記錄。 因為每個新的記錄的`ProductID`大於每當它附加至方格的結尾新增新產品的最後一個值。 因此，您可能要自動將使用者傳送至 GridView 的最後一頁中，加入新的記錄之後變更。 這可藉由在呼叫之後新增下列程式碼行`ProductsDataSource.Insert()`在`RowCommand`以指出使用者必須要傳送的最後一個頁面之後的資料繫結至 GridView 的事件處理常式：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` 頁面層級的布林值變數，一開始會指派值為`False`。 在 GridView`DataBound`事件處理常式，如果`SendUserToLastPage`為 false，`PageIndex`屬性更新為將使用者傳送至最後一頁。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

原因`PageIndex`屬性會設定`DataBound`事件處理常式 (相對於`RowCommand`事件處理常式) 是因為當`RowCommand`事件處理常式，就會引發我們尚未加入至新的記錄 ve`Products`資料庫資料表。 因此，在`RowCommand`事件處理常式的最後一個頁面索引 (`PageCount - 1`) 代表最後一個頁面索引*之前*已新增新的產品。 對於大部分的要新增的產品，最後一個頁面索引是相同之後加入新的產品。 當新增的產品會導致新的最後一個頁面索引，如果我們不正確地更新，但`PageIndex`在`RowCommand`事件處理常式，然後我們將會進入第二個至最後一頁 （最後一個頁面索引才能再新增新的產品） 而不是新的最後一頁 index。 由於`DataBound`已新增新的產品，並將資料重新繫結到方格中，藉由設定之後，就會引發事件處理常式`PageIndex`那里的屬性我們知道我們重新取得正確的最後一個頁面索引。

最後，本教學課程使用 GridView 會變寬，由於必須針對加入新的產品中收集的欄位數目。 由於這個寬度，DetailsView s 垂直版面配置可能會偏好的。 GridView s 可收集較少的輸入，藉以降低整體的寬度。 我們或許不 t 需要收集`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位時加入新的產品，在此情況下無法移除這些欄位從 GridView。

若要調整所收集的資料，我們可以使用下列方法之一：

- 繼續使用`AddProduct`預期值的方法`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位。 在 `Inserting`事件處理常式中，提供硬式編碼，預設要使用這些已移除了插入介面的輸入值。
- 建立新的多載`AddProduct`方法中的`ProductsBLL`不接受輸入的類別`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位。 然後，在 ASP.NET 頁面中，設定要使用這個新的多載的 ObjectDataSource。

其中一個選項同樣也會運作。 在過去的教學課程我們使用後面這個選項，建立多個多載`ProductsBLL`類別的`UpdateProduct`方法。

## <a name="summary"></a>總結

GridView 缺少 DetailsView 和 FormView 中找到的內建插入功能，但不少功夫插入介面可加入到頁尾資料列。 若要只顯示在 GridView 的頁尾資料列設定其`ShowFooter`屬性設`True`。 頁尾資料列內容可以自訂每個欄位轉換為 TemplateField 欄位和加入插入介面`FooterTemplate`。 當您在本教學課程中，我們看到`FooterTemplate`可以包含按鈕、 文字方塊、 dropdownlist 進行中，核取方塊，用來填入資料導向 Web 控制項 （例如 dropdownlist 進行），資料來源控制項和驗證控制項。 以及用來收集使用者 s 的輸入控制項，加入按鈕、 LinkButton、 ImageButton 需要或。

[新增] 按鈕按一下的時，ObjectDataSource 的`Insert()`啟動插入工作流程會叫用方法。 ObjectDataSource 會接著呼叫設定的 insert 方法 (`ProductsBLL`類別的`AddProduct`方法，在本教學課程)。 我們必須將值複製從 GridView s 插入到 ObjectDataSource 介面`InsertParameters`之前叫用插入方法的集合。 這可藉由程式設計的方式參考插入介面 Web 控制項中之 ObjectDataSource`Inserting`事件處理常式。

本教學課程中完成我們看看增強 GridView 外觀的技術。 如何使用二進位資料，例如影像、 Pdf、 Word 文件，並依此類推，以及資料 Web 控制項，會檢查下的一個教學課程集。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Bernadette Leigh。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](adding-a-gridview-column-of-checkboxes-vb.md)
