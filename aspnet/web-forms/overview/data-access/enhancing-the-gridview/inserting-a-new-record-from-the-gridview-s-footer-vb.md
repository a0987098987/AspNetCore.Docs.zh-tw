---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: 插入新的記錄從 GridView 的頁尾 (VB) |Microsoft 文件
author: rick-anderson
description: 雖然 GridView 控制項未提供插入新的記錄資料的內建支援，本教學課程會示範如何擴充以包含 GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 32f3cb23805813135bf463720e7479f5f819deb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888366"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>從 GridView 的頁尾 (VB) 插入新的記錄
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe)或[下載 PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> 雖然 GridView 控制項未提供插入新的記錄資料的內建支援，本教學課程會示範如何擴充以包含插入介面 GridView。


## <a name="introduction"></a>簡介

中所述[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程，GridView、 DetailsView 和 FormView Web 控制項每個包含內建的資料修改功能。 宣告式的資料來源控制項搭配使用時，這三個 Web 控制項可以快速且輕鬆地設定修改的資料集，而不需要撰寫一行程式碼的案例中。 不幸的是，只在 DetailsView 和 FormView 控制項所提供的內建插入、 編輯和刪除功能。 在 GridView 只提供編輯和刪除的支援。 不過，具有極小折線油漬，我們可以加強 GridView 加入插入的介面。

在 GridView 中加入插入的功能，我們必須負責決定將加入新的記錄、 建立插入的介面，以及撰寫程式碼來插入新的記錄項目。 在此教學課程中我們將探討將插入的介面加入至 GridView 的頁尾資料列 （請參閱圖 1）。 每個資料行的頁尾資料格包含適當的資料收集使用者介面項目 （產品 s 名稱文字方塊中，DropDownList 供應商，依此類推）。 我們也需要資料行新增按鈕，在按下時，將導致回傳，並插入新的記錄，到`Products`資料表頁尾資料列中提供的值。


[![頁尾資料列會提供介面以加入新的產品](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**圖 1**： 頁尾資料列加入新的產品中提供的介面 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>步驟 1： 在 GridView 中顯示產品資訊

我們考量自己建立 GridView 的頁尾中插入介面之前，可讓 s 專注 GridView 加入列出資料庫中的產品頁面第一次。 先開啟`InsertThroughFooter.aspx`頁面`EnhancedGridView`資料夾，然後從 [工具箱] 拖曳至設計工具，設定 GridView 的拖曳 GridView`ID`屬性`Products`。 接下來，使用 GridView s 智慧標籤繫結到名為新 ObjectDataSource `ProductsDataSource`。


[![建立名為 ProductsDataSource 新 ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`ProductsDataSource`([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


設定用於 ObjectDataSource`ProductsBLL`類別的`GetProducts()`方法來擷取產品資訊。 本教學課程，可讓 s 焦點，嚴格上加入插入的功能，不需擔心編輯和刪除。 因此，請確定在 [插入] 索引標籤中的下拉式清單設`AddProduct()`而更新和刪除索引標籤中的下拉式清單會設定為 （無）。


[![將 AddProduct 方法對應到 ObjectDataSource s insert （） 方法](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**圖 3**： 對應`AddProduct`ObjectDataSource s 方法`Insert()`方法 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![設定為 （無） 更新和刪除索引標籤的下拉式清單](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**圖 4**： 將更新和刪除索引標籤下拉式清單會列出為 （無） ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


完成 ObjectDataSource s 設定資料來源精靈之後，Visual Studio 會自動將欄位加入至 GridView 每個對應的資料欄位。 現在，請保留所有欄位加入由 Visual Studio。 稍後在本教學課程，我們會再回來移除的部分欄位的值不需要加入新的記錄時指定。

由於有接近 80 產品在資料庫中，使用者必須一路到網頁底部捲動才能加入新的記錄。 因此，可讓 s 啟用分頁，使插入的介面，更可見和可存取。 若要開啟分頁，只會檢查 GridView s 智慧標籤啟用分頁核取方塊。

此時，GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![產品的所有資料欄位會都顯示在分頁的 GridView](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**圖 5**： 產品的所有資料欄位會都顯示在分頁的 GridView ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>步驟 2： 加入頁尾資料列

其標頭和資料列，以及 GridView 包含頁尾資料列。 頁首和頁尾資料列會顯示依據的 GridView 的值[ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx)和[ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx)屬性。 若要顯示頁尾資料列，只要設定`ShowFooter`屬性`True`。 如圖 6 所示，設定`ShowFooter`屬性`True`方格中加入頁尾資料列。


[![若要顯示頁尾資料列，將 ShowFooter 設定為 True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**圖 6**： 若要顯示頁尾資料列中，設定`ShowFooter`至`True`([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


請注意頁尾資料列有深紅色背景的色彩。 這是因為我們建立並套用至所有頁面 DataWebControls 佈景主題回[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程。 具體來說，`GridView.skin`檔案會設定`FooterStyle`這類屬性，這個屬性會使用`FooterStyle`CSS 類別。 `FooterStyle`類別定義於`Styles.css`，如下所示：


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> 我們已瀏覽先前的教學課程中使用的 GridView 的頁尾資料列。 如有需要請參閱上一步[GridView 的頁尾中顯示摘要資訊](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)教學課程的重新整理器。


設定之後`ShowFooter`屬性`True`，請花一點時間瀏覽器中檢視輸出。 目前的頁尾資料列不 t 會包含任何文字或 Web 控制項。 在步驟 3 中，我們將修改 GridView 的每個欄位的頁尾，使其包含插入適當的介面。


[![空的頁尾資料列會顯示上面分頁介面控制項](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**圖 7**: 空頁尾資料列會顯示上面分頁介面控制項 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>步驟 3： 自訂頁尾資料列

回到[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教學課程中我們可了解如何大幅自訂特定 GridView 資料行使用 TemplateFields （相對於 BoundFields 或 CheckBoxFields） 的顯示方式; 在[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)我們探討了使用 TemplateFields 自訂 GridView 中的編輯介面。 回收 TemplateField 組成的範本數，定義標記、 Web 控制項與資料繫結語法的混合使用特定類型的資料列。 `ItemTemplate`，例如，可以指定的範本用於唯讀的資料列，雖然`EditItemTemplate`定義範本，可編輯的資料列。

連同`ItemTemplate`和`EditItemTemplate`，也包含 TemplateField`FooterTemplate`指定頁尾資料列的內容。 因此，我們可以將新增所需的每個欄位 s，插入至介面的 Web 控制項`FooterTemplate`。 若要開始，請將所有的 GridView 中的欄位轉換成 TemplateFields。 這可藉按一下編輯資料行中的連結 GridView s 智慧標籤，在左下角，選取每個欄位，然後按一下 轉換此欄位為 TemplateField 連結。


![將每個欄位轉換為 TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**圖 8**： 將每個欄位轉換為 TemplateField


按一下 轉換 TemplateField 到這個欄位會變成對等的 TemplateField 到目前的欄位類型。 例如，具有為 TemplateField 會取代每個 BoundField `ItemTemplate` ，其中包含會顯示對應的資料欄位的標籤和`EditItemTemplate`會顯示在文字方塊中的資料欄位。 `ProductName` BoundField 都已轉換成下列 TemplateField 標記：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

同樣地， `Discontinued` CheckBoxField 轉換為 TemplateField 其`ItemTemplate`和`EditItemTemplate`包含核取方塊 Web 控制項 (使用`ItemTemplate`s 核取方塊已停用)。 唯讀`ProductID`BoundField 都已轉換成標籤控制項中都具有為 TemplateField`ItemTemplate`和`EditItemTemplate`。 簡單地說，轉換現有的 GridView TemplateField 欄位是功能的可快速又簡單地切換至更可自訂的 TemplateField，而不會遺失任何現有的欄位。

因為 GridView 我們重新使用規定 t 支援編輯，則可以自由移除`EditItemTemplate`只從每個 TemplateField，離開`ItemTemplate`。 這麼做以後 GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

既然 GridView 的每個欄位都已轉換成 TemplateField，我們可以輸入適當的插入介面到每個欄位的`FooterTemplate`。 某些欄位不會插入介面 (`ProductID`、 執行個體); 其他項目會用來收集新的產品的資訊的 Web 控制項中而有所不同。

若要建立編輯介面，選擇 編輯樣板連結從 GridView s 智慧標籤。 然後，從下拉式清單中，選取適當的欄位的`FooterTemplate`並從 [工具箱] 拖曳至設計工具拖曳適當的控制項。


[![將插入適當的介面加入至每個欄位的 FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**圖 9**： 將適當的插入介面加入至每個欄位 s `FooterTemplate` ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


下列項目符號清單列舉 GridView 欄位，指定要新增插入介面：

- `ProductID` 無。
- `ProductName` 加入文字方塊，並設定其`ID`至`NewProductName`。 若要確定使用者輸入的值，新的產品 s 名稱加入 RequiredFieldValidator 控制項。
- `SupplierID` 無。
- `CategoryID` 無。
- `QuantityPerUnit` 加入文字方塊中，設定其`ID`至`NewQuantityPerUnit`。
- `UnitPrice` 加入名為文字方塊`NewUnitPrice`CompareValidator，以確保輸入的值是貨幣值大於或等於零。
- `UnitsInStock` 使用文字方塊的`ID`設`NewUnitsInStock`。 包含 CompareValidator，以確保輸入的值是大於或等於零的整數值。
- `UnitsOnOrder` 使用文字方塊的`ID`設`NewUnitsOnOrder`。 包含 CompareValidator，以確保輸入的值是大於或等於零的整數值。
- `ReorderLevel` 使用文字方塊的`ID`設`NewReorderLevel`。 包含 CompareValidator，以確保輸入的值是大於或等於零的整數值。
- `Discontinued` 加入核取方塊，設定其`ID`至`NewDiscontinued`。
- `CategoryName` 加入 DropDownList 並設定其`ID`至`NewCategoryID`。 繫結到名為新 ObjectDataSource`CategoriesDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategories()`方法。 具有 DropDownList s `ListItem` s 顯示`CategoryName`資料欄位中，使用`CategoryID`做為其值的資料欄位。
- `SupplierName` 加入 DropDownList 並設定其`ID`至`NewSupplierID`。 繫結到名為新 ObjectDataSource`SuppliersDataSource`並將它設定為使用`SuppliersBLL`類別的`GetSuppliers()`方法。 具有 DropDownList s `ListItem` s 顯示`CompanyName`資料欄位中，使用`SupplierID`做為其值的資料欄位。

針對每個驗證控制項中，清除`ForeColor`屬性以便`FooterStyle`CSS 類別 s 白色的前景色彩將會使用以取代預設紅色。 也使用`ErrorMessage`屬性的詳細說明，但設定`Text`星號的屬性。 若要避免驗證控制項的文字導致插入的介面，以包裝到兩行，將`FooterStyle`s`Wrap`屬性設定為 false 的每個`FooterTemplate`的驗證控制項。 最後，會加入 ValidationSummary 控制項下方的 GridView 和設定其`ShowMessageBox`屬性`True`及其`ShowSummary`屬性`False`。

當加入新的產品，我們需要提供`CategoryID`和`SupplierID`。 透過在頁尾儲存格，DropDownLists 會擷取此資訊`CategoryName`和`SupplierName`欄位。 我選擇要使用這些欄位相`CategoryID`和`SupplierID`TemplateFields 因為 s 方格中資料列的使用者是有可能更有興趣查看的類別目錄和供應商名稱，而不是其識別碼值。 因為`CategoryID`和`SupplierID`值現在正在擷取中`CategoryName`和`SupplierName`欄位 s 插入介面，我們可以移除`CategoryID`和`SupplierID`TemplateFields 從 GridView。

同樣地，`ProductID`時加入新的產品，不會使用所以`ProductID`TemplateField 可以一併移除。 不過，可以讓 s 保留`ProductID`方格中的欄位。 除了文字方塊、 DropDownLists、 核取方塊及驗證的控制項，可以插入介面組成，我們還需要新增按鈕，按一下時，會執行邏輯，以將新的產品加入資料庫。 步驟 4 中，我們將包含插入的介面中的 [新增] 按鈕`ProductID`TemplateField 的`FooterTemplate`。

請隨意改善各個 GridView 欄位的外觀。 例如，您可能想要格式化`UnitPrice`為貨幣值靠右對齊`UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`欄位，以及更新`HeaderText`TemplateFields 的值。

建立插入介面中的時光靠之後`FooterTemplate`s，移除`SupplierID`，和`CategoryID`TemplateFields，並改善透過格式和對齊 TemplateFields，宣告您 GridView 的方格的美學標記看起來應該如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

當透過瀏覽器檢視，GridView 的頁尾資料列現在會包含已完成插入介面 （請參閱圖 10）。 此時，插入介面不包含使用者指出她 s 輸入資料的新產品，而且想要將新記錄插入資料庫的方法。 此外，我們尚未為了解決如何輸入頁尾的資料將會轉譯為中的新記錄 ve`Products`資料庫。 我們將探討如何加入 [新增] 按鈕插入介面，以及如何在執行程式碼的步驟 4 中回傳時該按下 s。 步驟 5 會示範如何插入新記錄使用頁尾中的資料。


[![GridView 頁尾加入新的記錄提供的介面](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**圖 10**: GridView 頁尾提供介面以加入新的記錄 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>步驟 4： 在插入介面包括 [新增] 按鈕

我們需要包含加入按鈕某處插入介面在因為目前插入介面頁尾資料列 s 缺少使用者，指出他們已完成輸入新的產品的資訊的方式。 這可以架設在其中一個現有`FooterTemplate`s 或我們無法將新的資料行加入方格針對此目的。 本教學課程，讓 s 放置中的 [新增] 按鈕`ProductID`TemplateField 的`FooterTemplate`。

從設計工具中，按一下 編輯範本中的連結 GridView s 智慧標籤，然後選擇`ProductID`欄位的`FooterTemplate`從下拉式清單。 新增按鈕 Web 控制項 （或 LinkButton 或 ImageButton，如果您想要的話） 範本，若要設定其識別碼`AddProduct`、 其`CommandName`至插入、 及其`Text`增益圖 11 中所示的屬性。


[![[新增] 按鈕置於 ProductID TemplateField 的 FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**圖 11**： 放置在 [新增] 按鈕`ProductID`TemplateField s `FooterTemplate` ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


一旦您已包含 [新增] 按鈕，測試出瀏覽器中。 請注意，當具有無效的資料插入介面中的 [新增] 按鈕簡單地說 circuited 回傳到 ValidationSummary 控制項表示無效的資料 （請參閱圖 12）。 輸入適當的資料，按一下 [新增] 按鈕，將會導致回傳。 沒有記錄會加入至資料庫，不過。 我們必須撰寫的實際執行插入的程式碼。


[![加入按鈕 s 的回傳正簡短 Circuited 如果插入介面中有無效的資料](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**圖 12**: 加入按鈕的回傳是簡短 Circuited 如果插入介面中有無效的資料 ([按一下以檢視完整大小的影像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> 驗證控制項插入介面中的不會指派給驗證群組。 這會正常運作，只要插入介面是唯一的驗證頁面上的控制項集合。 不過，還有其他驗證控制項 （例如格線 s 編輯介面中的驗證控制項） 頁面上，如果驗證控制項中插入介面，並加入按鈕的`ValidationGroup`屬性應該指派相同的值 so 與將這些控制項與特定的驗證群組產生關聯。 請參閱[將 ASP.NET 2.0 中的驗證控制項的分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)如需有關驗證群組中分割的驗證控制項和網頁上的按鈕。


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>步驟 5： 插入新的記錄，到`Products`資料表

當利用內建的編輯功能的 GridView，GridView 會自動處理的所有工作所需執行更新。 特別的是，當按一下 [更新] 按鈕會複製從編輯介面進入 ObjectDataSource s 中的參數值`UpdateParameters`集合和關閉叫用 ObjectDataSource 的更新是否已盡力而為`Update()`方法。 由於 GridView 不提供插入此類內建功能，我們必須實作程式碼呼叫 ObjectDataSource s`Insert()`方法，並複製的值插入介面 ObjectDataSource s`InsertParameters`集合.

已按下 [新增] 按鈕之後，應該執行此 insert 邏輯。 中所述[加入，而且回應 GridView 中的按鈕](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md)教學課程中，每當按鈕、 LinkButton 或在 GridView 的 imagebutton 已按下時，GridView 的`RowCommand`回傳事件時引發。 將按鈕、 LinkButton 或 ImageButton 已加入明確例如頁尾資料列中的 新增 按鈕，或如果它已自動加入，由 GridView，就會引發這個事件 (例如選取啟用排序時，每個資料行頂端 LinkButtons 或LinkButtons 中選取 啟用分頁時的分頁介面)。

因此，若要回應使用者按一下 [新增] 按鈕，我們需要建立事件處理常式 GridView s`RowCommand`事件。 因為每次引發此事件*任何*按鈕、 LinkButton 或在 GridView 的 imagebutton 已按下時，它很重要，我們才繼續執行具有插入邏輯的 s`CommandName`屬性傳遞至事件處理常式對應`CommandName` (Insert) 上的 [新增] 按鈕的值。 此外，我們也只應繼續如果驗證控制項報告有效的資料。 若要做到這一點，建立事件處理常式`RowCommand`為下列程式碼的事件：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> 您可能想知道為什麼事件處理常式困擾檢查`Page.IsValid`屬性。 畢竟，如果無效的資料插入介面中提供，被隱藏獲利的 t 回傳嗎？ 這項假設是正確的只要使用者未停用 JavaScript，或採取步驟來避免用戶端驗證邏輯。 簡單地說，其中一個永遠不應該依賴嚴格用戶端驗證。伺服器端的有效性應該一律執行檢查之前使用資料。


步驟 1 建立`ProductsDataSource`ObjectDataSource 使其`Insert()`方法對應到`ProductsBLL`類別的`AddProduct`方法。 若要插入新的記錄，到`Products`資料表中，我們只叫用 ObjectDataSource 的`Insert()`方法：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

既然`Insert()`叫用方法，所有會維持為從插入介面的值複製到參數傳遞給`ProductsBLL`類別的`AddProduct`方法。 如我們所見回[檢查插入、 更新和刪除與事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教學課程，這可藉由 ObjectDataSource 的`Inserting`事件。 在`Inserting`我們需要以程式設計方式參考的控制項，從事件`Products`GridView 的頁尾資料列，並指派其值`e.InputParameters`集合。 當使用者遺漏值，例如離開`ReorderLevel`我們需要指定應插入至資料庫的值的文字方塊空白`NULL`。 因為`AddProducts`方法會接受可為 null 的類型，可為 null 的資料庫欄位，只要使用可為 null 的類型，並將其值設定為`Nothing`在其中省略使用者輸入的情況下。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

與`Inserting`事件處理常式已完成，新的記錄可以加入至`Products`透過 GridView 的頁尾資料列的資料庫資料表。 請繼續並再試一次加入數個新的產品。

## <a name="enhancing-and-customizing-the-add-operation"></a>增強和自訂加入作業

目前，按一下 [新增] 按鈕加入至資料庫資料表的新記錄，但並不提供任何類型的視覺化回饋記錄已成功加入。 在理想情況下，標籤 Web 控制項或用戶端警示方塊會通知使用者，其插入已順利完成。 我將保留此為一項工作讀取器。

GridView 本教學課程中使用則不適用的任何排序次序列出的產品，也無法讓使用者排序資料。 因此，記錄會排序其主索引鍵欄位由資料庫中。 因為每個新的記錄有`ProductID`大於每次跟在方格的結尾加入新的產品的最後一個值。 因此，您可能想要自動傳送給使用者最後一頁的 GridView 加入新的記錄之後。 這可藉由呼叫之後加入下列程式碼行`ProductsDataSource.Insert()`中`RowCommand`事件處理常式，以指出使用者必須傳送到最後一個頁面後繫結至 GridView 的資料：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` 頁面層級的布林值變數，一開始是指派的值`False`。 在 GridView s`DataBound`事件處理常式，如果`SendUserToLastPage`為 false，`PageIndex`會更新屬性以傳送給使用者最後一頁。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

原因`PageIndex`屬性設定`DataBound`事件處理常式 (與`RowCommand`事件處理常式) 是因為當`RowCommand`事件處理常式就會引發我們尚未加入至新的記錄 ve`Products`資料庫資料表。 因此，在`RowCommand`事件處理常式的最後一個頁面的索引 (`PageCount - 1`) 代表最後一個頁面索引*之前*已加入新的產品。 對於大部分的產品加入，最後一個頁面索引是相同之後加入新的產品。 但當加入的產品產生新的最後一個頁面索引，如果未正確更新`PageIndex`中`RowCommand`事件處理常式，然後我們將會進入第二個到最後一頁 （最後一個頁面索引才能再新增新的產品） 而不是新的最後一頁 index。 因為`DataBound`已加入新的產品，並將資料重新繫結至方格中，藉由設定之後，就會引發事件處理常式`PageIndex`那里的屬性我們知道我們重新取得正確的最後一個頁面索引。

最後，在本教學課程中使用的 GridView 是很寬，由於必須針對加入新的產品收集的欄位數目。 由於此寬度，DetailsView s 垂直配置可能會偏好。 GridView s 可收集較少的輸入，藉以降低整體的寬度。 我們不收集 t 需要可能`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位時加入新的產品，在此情況下無法從 GridView 移除這些欄位。

若要調整所收集的資料，我們可以使用兩種方法的其中一個：

- 繼續使用`AddProduct`預期值的方法`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位。 在`Inserting`事件處理常式，提供硬式編碼，預設要使用已插入介面從這些輸入值。
- 建立新的多載`AddProduct`方法中的`ProductsBLL`不接受輸入的類別`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`欄位。 然後，在 ASP.NET 頁面中，設定要使用這個新的多載 ObjectDataSource。

其中一個選項同樣也會運作。 在過去的教學課程我們使用第二種選項，建立多個多載`ProductsBLL`類別的`UpdateProduct`方法。

## <a name="summary"></a>總結

在 GridView 缺少 DetailsView 和 FormView 中的內建插入功能，但投入時間的 24 位元插入介面可以加入頁尾資料列。 若要只顯示頁尾資料列在 GridView 中設定其`ShowFooter`屬性`True`。 可以藉由將欄位轉換為 TemplateField 針對每個欄位自訂頁尾資料列內容，並新增插入介面`FooterTemplate`。 當我們在本教學課程，了解`FooterTemplate`只能包含按鈕、 文字方塊、 DropDownLists、 核取方塊，用來填入資料導向 Web 控制項 （例如 DropDownLists) 的資料來源控制項和驗證控制項。 以及用來收集使用者 s 的輸入控制項，加入按鈕、 LinkButton，ImageButton 需要或。

[新增] 按鈕按一下的時，ObjectDataSource 的`Insert()`啟動插入工作流程會叫用方法。 ObjectDataSource 然後會呼叫設定的插入方法 (`ProductsBLL`類別的`AddProduct`方法，在本教學課程)。 我們必須將值複製從 GridView s 插入介面 ObjectDataSource s`InsertParameters`之前叫用插入方法的集合。 這可以透過程式設計方式參考 ObjectDataSource s 中的插入介面 Web 控制項`Inserting`事件處理常式。

本教學課程完成我們查看強化 GridView 的外觀的技術。 如何使用二進位資料，例如影像、 Pdf、 Word 文件，依此類推，以及 Web 控制項的資料，會檢查下的一個教學課程集。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Bernadette Leigh。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](adding-a-gridview-column-of-checkboxes-vb.md)
