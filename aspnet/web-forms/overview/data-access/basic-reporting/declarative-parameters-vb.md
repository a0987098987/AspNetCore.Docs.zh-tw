---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: 宣告式的參數 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將說明如何使用參數設定為硬式編碼值來選取要在 DetailsView 控制項中顯示的資料。
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 01330d6c743fa9534cdb5dfa42bde5dbbe954c40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839225"
---
<a name="declarative-parameters-vb"></a>宣告式的參數 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe)或[下載 PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> 在本教學課程中，我們將說明如何使用參數設定為硬式編碼值來選取要在 DetailsView 控制項中顯示的資料。


## <a name="introduction"></a>簡介

在 [最後一個教學課程](displaying-data-with-the-objectdatasource-vb.md)我們探討使用 GridView、 DetailsView 和 FormView 控制項繫結至叫用的 ObjectDataSource 控制項顯示資料`GetProducts()`方法從`ProductsBLL`類別。 `GetProducts()`方法會傳回強型別的 DataTable，填入來自 Northwind 資料庫的記錄的所有`Products`資料表。 `ProductsBLL`類別包含的產品-只傳回子集的其他方法`GetProductByProductID(productID)`， `GetProductsByCategoryID(categoryID)`，和`GetProductsBySupplierID(supplierID)`。 這三種方法預期的輸入的參數，指出如何篩選傳回的產品資訊。

ObjectDataSource 可用來叫用方法預期的輸入的參數，但若要這樣做就必須指定這些參數的值來自何處。 參數值可以是硬式編碼，或可能來自各種不同的動態來源，包括： 查詢字串值，工作階段變數，在頁面上，或其他項目上的 Web 控制項的屬性值。

在本教學課程一開始先說明如何使用參數，設定為硬式編碼的值。 具體來說，我們將探討加入顯示特定產品，也就是 Chef Anton Gumbo 混合，其具有的相關資訊的網頁的 DetailsView`ProductID`為 5。 接下來，我們將了解如何設定 Web 控制項為基礎的參數值。 特別的是，我們將使用一個文字方塊，讓使用者輸入的國家/地區之後，他們可以按一下按鈕以查看該國家/地區中的供應商的清單。

## <a name="using-a-hard-coded-parameter-value"></a>使用硬式編碼的參數值

第一個範例中，開始將 DetailsView 控制項加入`DeclarativeParams.aspx`頁面中`BasicReporting`資料夾。 從 DetailsView 的智慧標籤，選取&lt;新的資料來源&gt;從下拉式清單並選擇新增 ObjectDataSource。


[![加入至頁面的 ObjectDataSource](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**圖 1**： 新增至頁面的 ObjectDataSource ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image3.png))


這會自動啟動 ObjectDataSource 控制項的 [選擇資料來源] 精靈。 選取`ProductsBLL`類別精靈的第一個畫面。


[![選取 ProductsBLL 類別](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**圖 2**： 選取 `ProductsBLL`類別 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image6.png))


因為我們想要顯示特定產品的相關資訊，所以我們想要使用`GetProductByProductID(productID)`方法。


[![選擇 GetProductByProductID(productID) 方法](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**圖 3**： 選擇`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image9.png))


因為我們所選取的方法包含參數，還有一個更多的畫面精靈 」，我們就必須定義要用於參數的值。 在左側的清單會顯示所有選取方法的參數。 針對`GetProductByProductID(productID)`只有一個`productID`。 在右側中，我們可以指定所選取參數的值。 參數來源下拉式清單會列舉可能的各種來源，參數值。 因為我們想要指定硬式編碼的值為 5`productID`參數保留為 「 無參數來源和 [預設值] 文字方塊中輸入 5。


[![Hard-Coded 參數值的 5 用於 productID 參數](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**圖 4**: A Hard-Coded 參數值的 5 將會用於`productID`參數 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image12.png))


在精靈完成後設定資料來源，包括 ObjectDataSource 控制項宣告式標記`Parameter`物件`SelectParameters`集合中定義的方法所需的輸入參數的每個`SelectMethod`屬性。 因為我們在此範例中使用的方法必須要有只為單一輸入的參數， `parameterID`，這裡只有一個項目。 `SelectParameters`集合可以包含任何類別都衍生自`Parameter`類別中`System.Web.UI.WebControls`命名空間。 硬式編碼的參數值的基底`Parameter`會使用類別，但其他參數的來源選項衍生`Parameter`類別用; 您也可以建立您自己[自訂參數型別](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)，如有需要。

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> 如果您有依照上述指示在您自己的電腦上的宣告式標記您會看到在此時可能包含值`InsertMethod`， `UpdateMethod`，並`DeleteMethod`屬性，以及`DeleteParameters`。 ObjectDataSource 的 [選擇資料來源] 精靈會自動將指定的方法，從`ProductBLL`讓除非您明確地清除那些時，它們會包含於上述的標記，用於插入、 更新和刪除。


資料 Web 控制項時瀏覽此頁面，將會叫用的 ObjectDataSource`Select`方法，它會呼叫`ProductsBLL`類別的`GetProductByProductID(productID)`方法使用硬式編碼的值為 5`productID`輸入的參數。 此方法會傳回強型別`ProductDataTable`物件，其中包含 Chef Anton Gumbo 混合的相關資訊的單一資料列 (與產品`ProductID`5)。


[![顯示的資訊關於 Chef Anton 的 Gumbo 混合](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**圖 5**： 顯示的資訊關於 Chef Anton 的 Gumbo 混合 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>將參數值設定為 Web 控制項的屬性值

ObjectDataSource 的參數值也可以設定根據頁面上的 Web 控制項的值。 為了說明這點，讓我們的 GridView 會列出所有使用者所指定的國家/地區中的供應商。 若要完成本入門將文字方塊新增至 [使用者可以在其中輸入國家/地區名稱] 頁面。 設定這個 TextBox 控制項的`ID`屬性設`CountryName`。 也加入 Button Web 控制項。


[![將文字方塊加入至識別碼 CountryName 的頁面](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**圖 6**: [使用] 頁面中新增 TextBox `ID` `CountryName` ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image18.png))


接下來，將 GridView，加入頁面，以及從智慧標籤，選擇加入新的 ObjectDataSource。 因為我們想要顯示供應商資訊選取`SuppliersBLL`從精靈的第一個畫面的類別。 在第二個畫面中，挑選`GetSuppliersByCountry(country)`方法。


[![選擇 GetSuppliersByCountry(country) 方法](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**圖 7**： 選擇`GetSuppliersByCountry(country)`方法 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image21.png))


因為`GetSuppliersByCountry(country)`方法有輸入的參數，此精靈會再次包含選擇的參數值的最後一個畫面。 此時，設定參數來源控制項。 這將會填入頁面; 上的控制項名稱 ControlID 下拉式清單選取`CountryName`從清單中的控制項。 當第一次瀏覽的頁面`CountryName`文字方塊會是空白的因此會傳回任何結果，並不會顯示。 如果您想要依預設會顯示一些結果，請據以設定 [預設值] 文字方塊中。


[![設定 CountryName 控制項值的參數值](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**圖 8**： 將參數值設定為`CountryName`控制項的值 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image24.png))


ObjectDataSource 的宣告式標記會稍有不同第一個範例中，使用[ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx)而不是標準`Parameter`物件。 A`ControlParameter`有額外的屬性，以指定`ID`Web 控制項和要使用參數的屬性值 (`PropertyName`)。 設定資料來源精靈是聰明，若要判斷，針對文字方塊中，我們可能會想要使用`Text`參數值的屬性。 如果您想要使用不同的屬性值從 Web 控制項的不過，您可以變更`PropertyName`值以下，或按一下精靈上的 [顯示進階屬性] 連結。

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

第一次造訪網頁時`CountryName`文字方塊是空的。 ObjectDataSource`Select`方法會叫用由 GridView，而值為`Nothing`傳入`GetSuppliersByCountry(country)`方法。 將 TableAdapter`Nothing`到資料庫`NULL`值 (`DBNull.Value`)，但所使用的查詢`GetSuppliersByCountry(country)`會寫入方法，使它不會傳回任何值時`NULL`指定值`@CategoryID`參數。 簡單地說，就會不傳回任何供應商。

訪客進入國家/地區，不過，並按一下 [顯示供應商] 按鈕，以產生回傳，ObjectDataSource 的之後`Select`查詢方法，在 TextBox 控制項中傳遞`Text`的值`country`參數。


[![會顯示來自加拿大的這些供應商](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**圖 9**： 顯示那些來自供應商加拿大 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>顯示預設的所有供應商

而是超過第一次檢視頁面時顯示任何供應商我們可能會想要顯示*所有*供應商一開始，可讓使用者在文字方塊中輸入國家/地區名稱削減清單。 文字方塊是空的當`SuppliersBLL`類別的`GetSuppliersByCountry(country)`方法會傳入`Nothing`針對其*`country`* 輸入的參數。 這`Nothing`值接著會向下傳遞至 DAL`GetSupplierByCountry(country)`方法，它就會轉譯到資料庫`NULL`值`@Country`在下列查詢中的參數：

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

運算式`Country = NULL`一律會傳回 False，即使是針對記錄其`Country`資料行具有`NULL`值; 因此，會傳回任何記錄。

返回*所有*供應商的國家/地區文字方塊為空白時，我們可以加強`GetSuppliersByCountry(country)`方法來叫用到 BLL 中的`GetSuppliers()`方法，其國家/地區參數時`Nothing`並呼叫 DAL `GetSuppliersByCountry(country)`否則的方法。 這會有包含國家/地區參數時，傳回不指定任何國家/地區時，所有供應商以及供應商的適當子集的效果。

變更`GetSuppliersByCountry(country)`方法中的`SuppliersBLL`下列類別：

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

透過這項變更`DeclarativeParams.aspx`頁面會顯示所有的供應商，當第一次瀏覽 (或每當`CountryName`文字方塊是空的)。


[![所有的供應商會根據預設，現在顯示](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**圖 10**： 所有的供應商會現在顯示的預設值 ([按一下以檢視完整大小的影像](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>總結

若要使用輸入參數的方法，我們要指定參數的值中的 ObjectDataSource`SelectParameters`集合。 要從不同來源取得的參數值允許不同類型的參數。 預設參數類型會使用硬式編碼值，但可以從查詢字串、 工作階段變數、 cookie 和甚至是使用者輸入的值，從頁面上的 Web 控制項取得參數值一樣輕鬆地 （及不包含一行程式碼）。

我們討論過在本教學課程的範例說明如何使用宣告式的參數值。 不過，那里可能是當我們需要使用參數的來源未提供，例如目前的日期和時間，或者，如果我們的網站使用成員資格，造訪者的使用者識別碼。 對於這類情況我們可以設定參數值 ObjectDataSource 之前以程式設計方式叫用其基礎物件的方法。 我們會看到如何完成這[下一個教學課程](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-data-with-the-objectdatasource-vb.md)
> [下一頁](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
