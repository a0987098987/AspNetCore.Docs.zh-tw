---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: 在 DetailsView 控制項 (VB) 中使用 TemplateFields |Microsoft 文件
author: rick-anderson
description: 適用於 GridView 的相同 TemplateFields 功能也會提供與 DetailsView 控制項的。 在本教學課程中，我們會顯示一個產品...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 67009460477dcc3d1e966220b446a47d6e5b6f5a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>在 DetailsView 控制項 (VB) 中使用 TemplateFields
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe)或[下載 PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> 適用於 GridView 的相同 TemplateFields 功能也會提供與 DetailsView 控制項的。 本教學課程中我們將使用包含 TemplateFields DetailsView 一次顯示一個產品。


## <a name="introduction"></a>簡介

TemplateField 提供較高的轉譯資料的彈性比 BoundField、 CheckBoxField、 HyperLinkField，以及其他資料欄位的控制項。 在[上一個教學課程](using-templatefields-in-the-gridview-control-vb.md)我們探討了使用 TemplateField GridView 中：

- 顯示一個資料行中的多個資料欄位值。 具體來說，兩者`FirstName`和`LastName`欄位已合併為一個 GridView 的資料行。
- 您可以使用替代的 Web 控制項來表示資料欄位值。 我們了解如何顯示`HiredDate`值使用日曆控制項。
- 顯示基礎資料為基礎的狀態資訊。 雖然`Employees`資料表不包含的資料行，傳回員工已於作業的天數，我們能夠使用 TemplateField 和格式化的方法與上一個教學課程中，這項資訊顯示在 GridView 範例。

適用於 GridView 的相同 TemplateFields 功能也會提供與 DetailsView 控制項的。 本教學課程中我們將使用包含兩個 TemplateFields DetailsView 一次顯示一個產品。 第一個 TemplateField 會結合`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`到 DetailsView 列的資料欄位。 第二個 TemplateField 將顯示的值`Discontinued`欄位，但會使用格式化的方式來顯示 [是] 如果`Discontinued`是`True`，，否則 「 否 」。


[![兩個 TemplateFields 可用來自訂顯示](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**圖 1**： 兩個 TemplateFields 可用來自訂顯示 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


讓我們開始吧 ！

## <a name="step-1-binding-the-data-to-the-detailsview"></a>資料繫結至 DetailsView 步驟 1:

中所述上一個教學課程中，使用的 TemplateFields 通常最為簡單，首先建立包含只 BoundFields 的 DetailsView 控制項然後加入新 TemplateFields 或 TemplateFields 做為轉換成現有 BoundFields 時所需. 因此，透過設計工具頁面中加入 DetailsView 的繫結至會傳回一份產品的 Sqldatasource 開始本教學課程。 這些步驟將使用 BoundFields 建立 DetailsView 針對每個產品的非布林值欄位和一個布林值欄位 (Discontinued) 的 CheckBoxField。

開啟`DetailsViewTemplateField.aspx`頁面上，並從 [工具箱] 拖曳至設計工具拖曳 DetailsView。 在 DetailsView 的智慧標籤選擇以加入新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。


[![加入新的 ObjectDataSource 控制項叫用 GetProducts() 方法](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**圖 2**: ObjectDataSource 控制項新增該 Invokes`GetProducts()`方法 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


此報表中移除`ProductID`， `SupplierID`， `CategoryID`，和`ReorderLevel`BoundFields。 接下來，重新排列 BoundFields 以便`CategoryName`和`SupplierName`BoundFields 緊接`ProductName`BoundField。 則可以自由調整`HeaderText`適當屬性和為您 BoundFields 的格式化屬性。 像 GridView 中透過 [欄位] 對話方塊 （可編輯欄位中的連結在 DetailsView 的智慧標籤，即可存取） 或透過宣告式語法，可以執行這些 BoundField 層級編輯。 最後，清除 在 DetailsView 的`Height`和`Width`屬性值，以便讓 DetailsView 控制項加入展開根據顯示的資料，並選取智慧標籤的 啟用分頁核取方塊。

進行這些變更之後，DetailsView 控制項的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

花點時間檢視透過瀏覽器頁面。 此時您會看到單一產品列 (Chai) 以顯示產品的名稱、 類別、 供應商、 價格、 庫存、 順序，單位，以及其已停止的狀態的資料列。


[![使用一系列的 BoundFields 顯示產品的詳細資料](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**圖 3**: 產品詳細資料會顯示使用 BoundFields 數列 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>步驟 2： 將價格、 庫存量和訂購量結合成一個資料列

在 DetailsView 有一個資料列`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`欄位。 我們可以這些資料將欄位結合成單一資料列具有為 TemplateField 藉由新增新 TemplateField 或藉由轉換其中一個現有`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields TemplateField 到。 雖然我個人偏好轉換現有 BoundFields，讓我們藉由新增新 TemplateField 練習。

按一下編輯欄位中的連結即可啟動 [欄位] 對話方塊的 DetailsView 的智慧標籤來啟動。 接下來，加入新 TemplateField 並設定其`HeaderText`屬性設為 「 價格和清查 」 並讓它位於上方新 TemplateField 移動`UnitPrice`BoundField。


[![在 DetailsView 控制項中加入新 TemplateField](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**圖 4**: DetailsView 控制項中加入新 TemplateField ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


因為這個新 TemplateField 將會包含在目前顯示的值`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields，讓我們先移除它們。

此步驟中的最後一個工作是定義`ItemTemplate`價格和清查 TemplateField，它可以是標記完成透過編輯介面設計工具中，或以手動方式透過控制項的宣告式語法的 DetailsView 的範本。 如同 GridView 中可以存取在 DetailsView 的範本編輯介面上智慧標籤中的 [編輯樣板] 連結即可。 從這裡您可以選取要從下拉式清單中編輯，然後從 [工具箱] 加入任何 Web 控制項範本。

此教學課程中，開始將標籤控制項新增至價格與清查 TemplateField `ItemTemplate`。 接下來，按一下 編輯資料繫結中的連結標籤 Web 控制項的智慧標籤上，並繫結`Text`屬性`UnitPrice`欄位。


[![標籤的文字屬性繫結至 UnitPrice 資料欄位](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**圖 5**： 繫結的標籤`Text`屬性`UnitPrice`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>價格格式化為貨幣

此步驟中，標籤 Web 控制項價格和清查 TemplateField 現在會顯示所選產品的價格。 圖 6 為止顯示進度的螢幕擷取畫面，透過瀏覽器檢視時。


[![價格與清查 TemplateField 顯示價格](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**圖 6**: 價格與清查 TemplateField 顯示價格 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


請注意產品的價格不格式化為貨幣。 BoundField 格式也可以藉由設定`HtmlEncode`屬性`False`和`DataFormatString`屬性`{0:formatSpecifier}`。 TemplateField，不過，任何格式設定指示必須指定在資料繫結語法或使用應用程式的程式碼中 （例如在 ASP.NET 網頁的程式碼後置類別），定義於某處的格式化方法。

若要指定標籤 Web 控制項中所使用的資料繫結語法格式，傳回到 DataBindings 對話方塊即可編輯資料繫結中的連結標籤的智慧標籤。 您可以直接在 [格式] 下拉式清單中輸入的格式設定的指示，或選取其中一個定義的格式字串。 像使用 BoundField`DataFormatString`屬性的格式來指定`{0:formatSpecifier}`。

如`UnitPrice`欄位的使用貨幣格式指定選取適當的下拉式清單中的值，或輸入`{0:C}`以手動方式。


[![依據貨幣格式化價格](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**圖 7**： 格式化為貨幣價格 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


格式規格做為成第二個參數的指示以宣告方式，`Bind`或`Eval`方法。 只透過設計工具中的結果中的宣告式標記的下列資料繫結運算式中進行設定：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>將其餘的資料欄位加入至 TemplateField

現在我們已顯示並格式化`UnitPrice`資料的價格和清查 TemplateField 欄位，但仍然需要顯示`UnitsInStock`和`UnitsOnOrder`欄位。 讓我們來顯示這些該行下方價格和括號括住。 在設計工具中的範本編輯介面，可在這類標記新增定位程式範本內的資料指標只中，輸入要顯示的文字。 或者，您可以輸入這個標記中宣告式語法直接。

新增的 static 標記、 標籤 Web 控制項和資料繫結語法，這樣的價格和清查 TemplateField 顯示的價格和清查資訊就像這樣：

*UnitPrice*  
(**順序上 / 庫存：** *UnitsInStock* / *UnitsOnOrder*)

執行這項工作後 DetailsView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

這些變更，我們已合併為單一的 DetailsView 資料列的價格和清查資訊。


[![單一資料列中顯示的價格和清查資訊](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**圖 8**: 價格與清查資訊會顯示在單一資料列 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>步驟 3： 自訂的已停止的欄位資訊

`Products`資料表的`Discontinued`資料行是位元值，指出是否已停用產品。 當繫結至資料來源控制項的 DetailsView （或 GridView），布林值欄位中，就像`Discontinued`，而非布林值欄位，例如會實作為 CheckBoxFields `ProductID`， `ProductName`，依此類推，實作為BoundFields。 CheckBoxField 轉譯成資料欄位的值可為 True，並且未核取否則如果為核取的已停用核取方塊。

而非顯示 CheckBoxField 我們可能會想要顯示文字，指出已停止的產品。 若要完成此我們無法將 CheckBoxField 移除 DetailsView 並將 BoundField 其`DataField`屬性設定為`Discontinued`。 請花一點時間來執行這項操作。 這項變更後 DetailsView 顯示的文字"True"不再生產的產品和"False"仍在作用中的產品。


[![字串為 True 和 False 用來顯示已停止的狀態](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**圖 9**: 字串，則為 True 和 False 可用來顯示不再提供狀態 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


假設我們不想要使用"YES"和"NO"，但改為"True"或"False"字串。 這類自訂可以執行具有為 TemplateField 和格式化方法的協助。 格式化的方法可以接受任意數目的輸入參數，但必須回到 HTML （做為字串） 插入至範本。

將格式化的方法，加入`DetailsViewTemplateField.aspx`名為頁面的程式碼後置類別`DisplayDiscontinuedAsYESorNO`接受`Northwind.ProductsRow`物件做為輸入參數和傳回的字串。 上一個教學課程中，這個方法中所述*必須*標示為`Protected`或`Public`才能存取的範本。


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

這個方法會檢查輸入的參數 (`discontinued`)，如果它是傳回"YES" `True`，否則為"NO"。

> [!NOTE]
> 中的格式化方法，檢查先前的教學課程提醒您，我們已傳入的資料欄位，可能會包含`NULL`s，因此需要檢查，如果員工的`HiredDate`屬性值有資料庫`NULL`值之前存取`EmployeesRow`的`HiredDate`屬性。 因為不需要這類核取這裡`Discontinued`資料行永遠不會有資料庫`NULL`指派的值。 此外，這就是為什麼方法接受布林值輸入參數，而不是需要接受`ProductsRow`執行個體或類型的參數`Object`。


使用此完成的格式化方法，剩下的就是呼叫從 TemplateField `ItemTemplate`。 若要建立 TemplateField 移除`Discontinued`BoundField 並加入新 TemplateField 或轉換`Discontinued`BoundField TemplateField 到。 然後，從 [宣告式標記] 檢視中，編輯 TemplateField，使其包含只會叫用 ItemTemplate`DisplayDiscontinuedAsYESorNO`方法，傳入的目前值`ProductRow`執行個體的`Discontinued`屬性。 這可以透過存取`Eval`方法。 具體來說，TemplateField 標記看起來應該像：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

這會導致`DisplayDiscontinuedAsYESorNO`轉譯 DetailsView 時要叫用的方法傳入`ProductRow`執行個體的`Discontinued`值。 因為`Eval`方法會傳回型別的值`Object`，但`DisplayDiscontinuedAsYESorNO`方法預期類型的輸入的參數`Boolean`，我們轉換`Eval`方法會傳回值至`Boolean`。 `DisplayDiscontinuedAsYESorNO`方法接著會傳回"YES"或"NO"值而定接收。 傳回的值是這個 DetailsView 中顯示的內容 （請參閱圖 10） 的資料列。


[![Yes 或 NO 的值為此時不再提供資料列中顯示](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**圖 10**: YES 或 NO 值會顯示不再提供資料列中 ([按一下以檢視完整大小的影像](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>總結

在 DetailsView 控制項 TemplateField 允許較高的彈性比其他欄位控制項而且理想的情況下，顯示資料的位置：

- 需要一個 GridView 的資料行中顯示多個資料欄位
- 資料是最適合使用表示 Web 控制項，而不是純文字
- 輸出取決於基礎資料，例如顯示中繼資料，或在重新格式化資料

雖然 TemplateFields 允許方面有更大的彈性，呈現在 DetailsView 的基礎資料，請 DetailsView 輸出仍覺得有點 boxy 為每個欄位會轉譯為 HTML 中的資料列`<table>`。

在 FormView 控制項提供更大的彈性地設定轉譯的輸出。 在 FormView 不包含欄位，但是只一系列的範本 (`ItemTemplate`， `EditItemTemplate`，`HeaderTemplate`等等)。 我們會看到如何使用 FormView 我們下一個教學課程中完成的呈現的版面配置更多控制。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Dan Jagers。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-templatefields-in-the-gridview-control-vb.md)
> [下一頁](using-the-formview-s-templates-vb.md)
