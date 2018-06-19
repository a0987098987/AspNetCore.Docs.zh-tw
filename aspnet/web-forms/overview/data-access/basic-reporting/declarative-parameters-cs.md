---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: 宣告式的參數 (C#) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們將說明如何使用參數設為硬式編碼值來選取要在 DetailsView 控制項中顯示的資料。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 840630852d28f49f4f4387f1d2cc6b275b468fc2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875935"
---
<a name="declarative-parameters-c"></a>宣告式的參數 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe)或[下載 PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> 在本教學課程中，我們將說明如何使用參數設為硬式編碼值來選取要在 DetailsView 控制項中顯示的資料。


## <a name="introduction"></a>簡介

在[最後一個教學課程](displaying-data-with-the-objectdatasource-cs.md)我們探討了 GridView、 DetailsView 和 FormView 控制項繫結至叫用 ObjectDataSource 控制項與顯示資料`GetProducts()`方法從`ProductsBLL`類別。 `GetProducts()`方法會傳回所有來自 Northwind 資料庫的記錄填入強型別 DataTable`Products`資料表。 `ProductsBLL`類別包含產品中，只傳回子集的其他方法`GetProductByProductID(productID)`， `GetProductsByCategoryID(categoryID)`，和`GetProductsBySupplierID(supplierID)`。 這三個方法預期輸入的參數，指出如何篩選傳回的產品資訊。

ObjectDataSource 可以用於叫用方法所預期的輸入的參數，但若要這麼做，所以我們必須指定這些參數的值來自何處。 參數值可以是硬式編碼，或可能來自各種不同的動態來源，包括： 查詢字串值，工作階段變數的頁面中，或其他 Web 控制項的屬性值。

在此教學課程首先說明如何使用參數，設定為硬式編碼值。 具體來說，我們會探討顯示特定產品，也就是 Chef Anton Gumbo 混合，其具有相關資訊的頁面中加入 DetailsView`ProductID`為 5。 接下來，我們會了解如何設定 Web 控制項為基礎的參數值。 特別是，我們會使用文字方塊中，讓使用者輸入國家/地區之後, 他們就可以按一下按鈕以查看位於該國家/地區的供應商的清單。

## <a name="using-a-hard-coded-parameter-value"></a>使用硬式編碼的參數值

第一個範例中，開始將 DetailsView 控制項加入`DeclarativeParams.aspx`頁面`BasicReporting`資料夾。 從 DetailsView 的智慧標籤上，選取&lt;新的資料來源&gt;從下拉式清單，然後選擇要加入 ObjectDataSource。


[![ObjectDataSource 加入頁面](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**圖 1**: ObjectDataSource 加入頁面 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image3.png))


這會自動啟動 ObjectDataSource 控制項的 [選擇資料來源] 精靈。 選取`ProductsBLL`類別精靈的第一個畫面。


[![選取 ProductsBLL 類別](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**圖 2**： 選取`ProductsBLL`類別 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image6.png))


因為我們想要顯示特定產品的相關資訊，我們想要使用`GetProductByProductID(productID)`方法。


[![選擇 GetProductByProductID(productID) 方法](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**圖 3**： 選擇`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image9.png))


因為我們所選取的方法包括的參數，所以沒有一個精靈 」，我們就必須定義要用於參數的值的多個螢幕。 在左邊的清單會顯示所有選取方法的參數。 如`GetProductByProductID(productID)`只有一個`productID`。 在右側我們可以指定所選取參數的值。 參數來源下拉式清單會列舉針對參數值不同的可能來源。 因為我們想要指定為 5 的硬式編碼值`productID`參數保留為 [無參數來源和預設值] 文字方塊中輸入 5。


[![Hard-Coded 參數值的 5 將用於 productID 參數](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**圖 4**: A Hard-Coded 參數值的 5 將用於`productID`參數 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image12.png))


完成設定資料來源精靈之後，ObjectDataSource 控制項的宣告式標記包含`Parameter`物件存放至`SelectParameters`集合中定義的方法所預期的輸入參數的每個`SelectMethod`屬性。 在此範例中，我們會使用此方法必須要有只為單一輸入的參數，因為`parameterID`，這裡只能有一個項目。 `SelectParameters`集合可以包含任何類別衍生自`Parameter`類別`System.Web.UI.WebControls`命名空間。 針對硬式編碼的參數值的基底`Parameter`類別會使用，但其他參數來源選項衍生`Parameter`使用類別; 您也可以建立您自己[自訂參數型別](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)有需要。

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> 如果您您按照您自己電腦上的宣告式標記您會看到在此時可能包含值`InsertMethod`， `UpdateMethod`，和`DeleteMethod`屬性，以及`DeleteParameters`。 ObjectDataSource 選擇資料來源精靈會自動將指定的方法，從`ProductBLL`用於插入、 更新和刪除，因此除非您明確地清除那些時，它們會包含在上述的標記。


當造訪此頁，Web 控制項的資料將會叫用 ObjectDataSource`Select`方法，將會呼叫`ProductsBLL`類別的`GetProductByProductID(productID)`方法使用硬式編碼的值為 5`productID`輸入的參數。 方法會傳回強型別`ProductDataTable`物件，其中包含 Chef Anton Gumbo 混合的相關資訊的單一資料列 (與產品`ProductID`5)。


[![顯示的資訊關於 Chef Anton 的 Gumbo 混合](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**圖 5**： 顯示的資訊關於 Chef Anton 的 Gumbo 混合 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>將參數值設定為 Web 控制項的屬性值

ObjectDataSource 參數也可以設定的值為基礎的頁面上的 Web 控制項的值。 為了說明這點，讓我們先列出所有已位於使用者指定的國家/地區供應商的 GridView。 若要完成此開始的頁面讓使用者可以輸入國家/地區名稱中加入文字方塊。 設定此文字方塊控制項的`ID`屬性`CountryName`。 也新增按鈕 Web 控制項。


[![將文字方塊加入至識別碼 CountryName 的頁面](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**圖 6**： 加入至包含頁面的文字方塊`ID` `CountryName` ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image18.png))


接下來，將 GridView 加入至頁面，然後從智慧標籤，選擇要加入新 ObjectDataSource。 因為我們想要顯示供應商資訊選取`SuppliersBLL`類別從精靈的第一個畫面。 從第二個畫面中，選取`GetSuppliersByCountry(country)`方法。


[![選擇 GetSuppliersByCountry(country) 方法](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**圖 7**： 選擇`GetSuppliersByCountry(country)`方法 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image21.png))


因為`GetSuppliersByCountry(country)`方法都有輸入的參數，精靈會再次包含選擇的參數值的最後一個畫面。 此時，設定參數來源控制項。 這將會填入頁面; 上的控制項名稱 ControlID 下拉式清單選取`CountryName`從清單控制項。 當第一次瀏覽頁面時`CountryName`文字方塊中會是空白的所以會傳回任何結果，並不會顯示。 如果您想要依預設會顯示某些結果，請據以設定 [預設值] 文字方塊。


[![設定為 CountryName 控制項值的參數值](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**圖 8**： 將參數值設定為`CountryName`控制項的值 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image24.png))


ObjectDataSource 的宣告式標記稍有不同於第一個範例中，使用[ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx)而不是標準`Parameter`物件。 A`ControlParameter`有其他屬性來指定`ID`Web 控制項和要用於參數的屬性值 (`PropertyName`)。 設定資料來源精靈是聰明，可以判斷，針對文字方塊中，我們可能會想要使用`Text`參數值的屬性。 如果您想要使用不同的屬性值從 Web 控制項的不過，您可以變更`PropertyName`此處或按一下 「 顯示進階屬性 」 的連結，在精靈中的值。

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

當第一次瀏覽頁面`CountryName`文字方塊是空的。 ObjectDataSource`Select`仍由 GridView，而值為叫用方法`null`傳入`GetSuppliersByCountry(country)`方法。 TableAdapter 會將轉換`null`插入資料庫`NULL`值 (`DBNull.Value`)，但使用的查詢`GetSuppliersByCountry(country)`會寫入方法，使它不會傳回任何值時`NULL`指定值`@CategoryID`參數。 簡單地說，就會傳回供應商。

一旦訪客便會進入國家/地區，不過，並按一下顯示的供應商按鈕，可導致回傳，ObjectDataSource 的`Select`方法重新查詢，在文字方塊控制項中傳遞`Text`視為`country`參數。


[![會顯示從供應商](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**圖 9**： 會顯示那些來自供應商加拿大 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>顯示預設的所有供應商

而是先檢視網頁時顯示任何的供應項目比我們可能會想要顯示*所有*供應商在一開始，讓使用者在文字方塊中輸入國家/地區名稱削減清單。 文字方塊是空的當`SuppliersBLL`類別的`GetSuppliersByCountry(country)`方法傳入`null`值及其*`country`* 輸入的參數。 這`null`值接著會向下傳遞至 DAL`GetSupplierByCountry(country)`方法，它就會轉譯到資料庫`NULL`值`@Country`參數，在下列查詢：

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

運算式`Country = NULL`永遠傳回 False，甚至記錄其`Country`資料行具有`NULL`值; 因此，會傳回任何記錄。

要傳回*所有*供應商的國家/地區的文字方塊空白時，我們可以加強`GetSuppliersByCountry(country)`方法中叫用 BLL`GetSuppliers()`方法時的國家/地區參數是`null`並呼叫 DAL `GetSuppliersByCountry(country)`其他方法。 這會有包含國家 （地區） 參數時，傳回不指定任何國家/地區時，所有供應商以及供應商的適當子集的效果。

變更`GetSuppliersByCountry(country)`方法中的`SuppliersBLL`下列類別：

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

透過這項變更`DeclarativeParams.aspx`頁面會顯示所有供應商，當第一次瀏覽 (或每當`CountryName`文字方塊是空的)。


[![所有的供應商為何現在根據預設顯示](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**圖 10**： 所有的供應商為何現在會顯示預設 ([按一下以檢視完整大小的影像](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>總結

若要使用具有輸入參數的方法，我們要指定參數的值在 ObjectDataSource`SelectParameters`集合。 要從不同來源取得的參數值允許不同類型的參數。 預設參數類型使用硬式編碼值，但可從查詢字串、 工作階段變數、 cookie 和即使使用者輸入的值，從網頁上的 Web 控制項取得參數值一樣輕鬆 （和沒有一行程式碼）。

我們探討了在本教學課程的範例將說明如何使用宣告式的參數值。 不過，可能是我們需要使用參數的來源不可以使用，例如目前的日期和時間，或者，如果我們的網站已使用成員資格，造訪者的使用者識別碼。 這種情況下我們可以設定參數值之前 ObjectDataSource 以程式設計方式叫用其基礎物件的方法。 我們會看到如何完成這項[下一個教學課程](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-data-with-the-objectdatasource-cs.md)
> [下一頁](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
