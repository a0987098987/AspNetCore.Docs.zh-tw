---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: 在 DetailsView 控制項 (C#) 中使用 TemplateFields |Microsoft Docs
author: rick-anderson
description: 使用 GridView，您可以使用相同的 TemplateFields 功能也可與 DetailsView 控制項。 在本教學課程中，我們會顯示一項產品...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 47c486737a3320bea631605621baac54dc6d257a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832032"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>在 DetailsView 控制項 (C#) 中使用 TemplateFields
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe)或[下載 PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> 使用 GridView，您可以使用相同的 TemplateFields 功能也可與 DetailsView 控制項。 在本教學課程中，我們會顯示一項產品使用包含 TemplateFields DetailsView 一次。


## <a name="introduction"></a>簡介

TemplateField 提供較高的轉譯資料的彈性比 BoundField、 CheckBoxField、 HyperLinkField 和其他資料欄位的控制項。 在 [先前的教學課程](using-templatefields-in-the-gridview-control-cs.md)我們探討了 GridView，以在使用 TemplateField:

- 顯示一個資料行中的多個資料欄位值。 具體而言，兩者`FirstName`和`LastName`欄位已合併為一個 GridView 資料行。
- 您可以使用替代的 Web 控制項來表示資料欄位值。 我們了解如何顯示`HiredDate`值使用日曆控制項。
- 顯示根據基礎資料的狀態資訊。 雖然`Employees`資料表不包含傳回的員工已在工作的天數的資料行中，我們能夠使用 TemplateField 和格式化的方法與先前的教學課程中，這項資訊顯示在 GridView 範例。

使用 GridView，您可以使用相同的 TemplateFields 功能也可與 DetailsView 控制項。 在本教學課程中，我們會顯示一項產品使用包含兩個 TemplateFields DetailsView 一次。 將合併的第一個 TemplateField `UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`進一 DetailsView 資料列的資料欄位。 值會顯示第二個 TemplateField`Discontinued`欄位中，但會使用"YES"時，要顯示的格式化方法`Discontinued`是`true`，「 否 」 則。


[![可用來自訂顯示兩個 TemplateFields](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**圖 1**： 用來自訂顯示兩個 TemplateFields ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


讓我們開始吧 ！

## <a name="step-1-binding-the-data-to-the-detailsview"></a>步驟 1: 資料繫結至 DetailsView

所述在先前的教學課程中使用的 TemplateFields 通常是最簡單的方式著手建立 DetailsView 控制項，其中包含只 BoundFields 然後新增新的 TemplateFields 或現有 BoundFields 轉換為 TemplateFields 時所需. 因此，開始本教學課程將 DetailsView 新增至透過設計工具頁面，並繫結至 ObjectDataSource 會傳回產品的清單。 這些步驟將會針對每個產品的非布林值欄位及其一個布林值欄位 (Discontinued)，以建立與 BoundFields DetailsView。

開啟`DetailsViewTemplateField.aspx`頁面上，然後從 [工具箱] 拖曳至設計工具拖曳 DetailsView。 從 DetailsView 的智慧標籤選擇 加入新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。


[![加入新的 ObjectDataSource 控制項叫用 GetProducts() 方法](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**圖 2**： 將新的 ObjectDataSource 控制項加入該 Invokes`GetProducts()`方法 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


此報表中移除`ProductID`， `SupplierID`， `CategoryID`，和`ReorderLevel`BoundFields。 接下來，重新排列 BoundFields 以便`CategoryName`並`SupplierName`BoundFields 緊接`ProductName`BoundField。 您可以自由調整`HeaderText`適當屬性和您 BoundFields 的格式化屬性。 像是 GridView，透過 [欄位] 對話方塊 （可編輯欄位中的連結 DetailsView 的智慧標籤，即可存取） 或透過宣告式語法，可以執行這些 BoundField 層級編輯。 最後，清除 DetailsView`Height`和`Width`屬性值，以便在 DetailsView 控制項加入展開根據顯示的資料，並檢查的智慧標籤的 啟用分頁核取方塊。

進行這些變更之後，您的 DetailsView 控制項宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

請花一點時間檢視透過瀏覽器頁面。 此時您應該看到列出單一的產品 (Chai) 與顯示的產品名稱、 類別、 供應商、 價格、 庫存單位數、 訂購，量和其已停止的狀態的資料列。


[![使用一系列的 BoundFields 顯示產品的詳細資料](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**圖 3**： 顯示使用 BoundFields 系列產品的詳細資料 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>步驟 2： 將價格、 庫存量和訂購量結合成一個資料列

DetailsView 有一個資料列`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`欄位。 我們可以結合這些資料欄位為單一資料列的 TemplateField 藉由新增新的 TemplateField 或藉由轉換其中一個現有`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields TemplateField 到。 雖然我個人比較偏好轉換現有 BoundFields，讓我們加入新的 TemplateField 練習。

在 DetailsView 的智慧標籤，即可啟動 欄位 對話方塊中的 編輯欄位 連結上按一下 啟動。 接下來，新增新的 TemplateField，並設定其`HeaderText`屬性設為 「 價格和清查 」 並移動新的 TemplateField 讓它位於上方`UnitPrice`BoundField。


[![在 DetailsView 控制項中加入新的 TemplateField](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**圖 4**： 在 DetailsView 控制項中加入新的 TemplateField ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


因為這個新的 TemplateField 會包含在目前顯示的值`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields，讓我們將它們移除。

此步驟中的最後一個工作是定義`ItemTemplate`價格和清查 TemplateField，它可以是標記來完成透過 DetailsView 中的樣板編輯介面設計工具中，或以手動方式透過控制項的宣告式語法。 如同 GridView、 DetailsView 的範本編輯介面可以存取中的智慧標籤的 [編輯範本] 連結上即可。 從這裡您可以選取要從下拉式清單中編輯，然後從 [工具箱] 新增任何 Web 控制項的範本。

本教學課程中，開始將標籤控制項新增至為價格與清查 TemplateField `ItemTemplate`。 接下來，按一下 [編輯資料繫結] 連結，從標籤 Web 控制項的智慧標籤，並繫結`Text`屬性設`UnitPrice`欄位。


[![Label 的 Text 屬性繫結至 UnitPrice 資料欄位](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**圖 5**： 繫結的標籤`Text`屬性設`UnitPrice`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>格式化為貨幣的價格

此步驟中，標籤 Web 控制項的價格和清查 TemplateField 現在會顯示剛才所選產品的價格。 圖 6 顯示進度的螢幕擷取畫面到目前為止透過瀏覽器檢視時。


[![價格與清查 TemplateField 顯示的價格](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**圖 6**: 價格與清查 TemplateField 顯示的價格 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


請注意，產品的價格不會格式化為貨幣。 BoundField 格式化是可以藉由設定`HtmlEncode`屬性，以`false`並`DataFormatString`屬性設`{0:formatSpecifier}`。 為 TemplateField，不過，任何格式化指示必須指定資料繫結語法中，或透過定義應用程式的程式碼 （例如 ASP.NET 頁面的程式碼後置類別中） 內的某處的格式化方法使用。

若要指定標籤 Web 控制項中所使用的資料繫結語法的格式化，回到資料繫結 對話方塊中按一下中標籤的智慧標籤的 編輯資料繫結 連結。 您可以直接在 [格式] 下拉式清單中輸入的格式設定的指示，或選取其中一個定義的格式字串。 如同 BoundField`DataFormatString`屬性，使用的格式指定`{0:formatSpecifier}`。

針對`UnitPrice`欄位的使用貨幣格式指定選取適當的下拉式清單值，或輸入`{0:C}`以手動方式。


[![格式化為貨幣的價格](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**圖 7**： 格式化成貨幣的價格 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


以宣告方式，在格式規格會表示為第二個參數插入`Bind`或`Eval`方法。 只要透過設計工具結果，在下列資料繫結運算式中的宣告式標記中設定：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>將其餘的資料欄位加入至 TemplateField

現在我們已顯示並格式化`UnitPrice`資料欄位中的價格和清查 TemplateField，但仍然需要顯示`UnitsInStock`和`UnitsOnOrder`欄位。 讓我們來顯示這些該行下方價格和括號括住。 從設計工具中的範本編輯介面，這類標記可以新增定位您在範本內的資料指標，然後只要輸入中要顯示的文字。 或者，您可以輸入此標記，直接在宣告式語法中。

新增靜態標記標籤 Web 控制項，資料繫結語法，以便為價格與清查 TemplateField 顯示的價格和清查資訊就像這樣：

*UnitPrice*  
(**庫存 / 訂單︰** *UnitsInStock* / *UnitsOnOrder*)

在執行這項工作之後 DetailsView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

經過這些變更中，我們已合併為單一的 DetailsView 資料列的價格和清查的資訊。


[![價格和清查資訊會顯示在單一資料列](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**圖 8**: 的價格和清查資訊會顯示在單一資料列 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>步驟 3： 自訂的已停止的欄位資訊

`Products`資料表的`Discontinued`資料行是位元值，指出是否已停用產品。 布林值欄位中，當繫結資料來源控制項 DetailsView （或 GridView），像是`Discontinued`，而非布林值欄位，例如，會實作為 CheckBoxFields `ProductID`， `ProductName`，依此類推，實作為BoundFields。 CheckBoxField 轉譯成資料欄位的值是否為 True，然後取消核取否則簽入已停用核取方塊。

而不是顯示 CheckBoxField 我們可能會想要改為顯示文字，指出產品已經停售。 若要這麼做，我們無法移除 DetailsView CheckBoxField，然後加入 BoundField 其`DataField`屬性設定為`Discontinued`。 請花一點時間來執行這項操作。 這項變更之後 DetailsView 顯示的文字"True"已停止的產品和"False"仍在作用中的產品。


[![字串為 True 和 False 用來顯示已停止的狀態](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**圖 9**: 字串，則為 True 和 False 可顯示 Discontinued 狀態 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


假設我們不想要使用"YES"和"NO"，但改為"True"或"False"字串。 利用 TemplateField 以及格式化的方法可以執行這類自訂。 可以接受任意數目的輸入參數，以格式化方法，但是 （做為字串） 必須傳回的 HTML 插入範本。

將格式化的方法，加入`DetailsViewTemplateField.aspx`頁面的程式碼後置類別名為`DisplayDiscontinuedAsYESorNO`接受布林值，做為輸入參數並傳回字串。 在上一個教學課程中，這個方法中所述*必須*標示為`protected`或`public`才能存取從該範本。


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

這個方法會檢查輸入的參數 (`discontinued`)，並傳回"YES"，如果它是`true`，否則為 [否]。

> [!NOTE]
> 檢查先前的教學課程回想一下，我們可能會包含資料欄位中所傳遞之格式化方法中`NULL`s，因此需要檢查是否員工`HiredDate`屬性值已有資料庫`NULL`值低於存取`EmployeesRow`的`HiredDate`屬性。 因為不需要這類的核取以下`Discontinued`資料行永遠不會有資料庫`NULL`指派的值。 此外，這就是為什麼方法接受布林值輸入參數，而不需要接受`ProductsRow`執行個體或類型參數的`object`。


使用此完整的格式化方法，剩下的就是呼叫從 TemplateField `ItemTemplate`。 若要建立 TemplateField 移除`Discontinued`BoundField 和新增新的 TemplateField，或轉換`Discontinued`BoundField TemplateField 到。 然後，從 [宣告式標記] 檢視中，編輯 TemplateField，使其包含只會叫用 ItemTemplate`DisplayDiscontinuedAsYESorNO`方法並傳入目前的值`ProductRow`執行個體的`Discontinued`屬性。 這可以透過存取`Eval`方法。 具體來說，TemplateField 標記應該看起來像：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

這會導致`DisplayDiscontinuedAsYESorNO`轉譯 DetailsView 時要叫用方法並傳遞`ProductRow`執行個體的`Discontinued`值。 由於`Eval`方法會傳回類型的值`object`，但`DisplayDiscontinuedAsYESorNO`方法所預期的輸入的參數的型別`bool`，我們轉換`Eval`方法會傳回值，以`bool`。 `DisplayDiscontinuedAsYESorNO`方法接著會傳回"YES"或"NO"的值而定接收。 傳回的值是在此 DetailsView 中顯示的內容 （請參閱 圖 10） 的資料列。


[![Yes 或 NO 的值為現在 Discontinued 資料列中顯示](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**圖 10**: YES 或 NO 的值為現在顯示 Discontinued 資料列 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>總結

在 DetailsView 控制項中的 TemplateField 允許較大程度的彈性比使用其他欄位控制項，並非常適合的情況下，顯示資料位置：

- 多個資料欄位需要一個 GridView 資料行中顯示
- 使用 Web 控制項，而不是純文字最能表示的資料
- 輸出取決於基礎資料，例如顯示中繼資料或是在重新格式化資料

雖然 TemplateFields 允許更高的 DetailsView 的基礎資料的呈現方式的彈性，DetailsView 輸出仍覺得有點 boxy 為每個欄位轉譯為 HTML 中的資料列`<table>`。

FormView 控制項提供更高的彈性地設定轉譯的輸出。 FormView 不包含欄位，但只是一系列的範本 (`ItemTemplate`， `EditItemTemplate`，`HeaderTemplate`等等)。 我們會看到如何使用 FormView 我們下一個教學課程中達成更多控制呈現的版面配置。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dan Jagers。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-templatefields-in-the-gridview-control-cs.md)
> [下一頁](using-the-formview-s-templates-cs.md)
