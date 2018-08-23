---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: 自訂 DataList 的編輯介面 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將建立更豐富的編輯介面的 DataList，當中包含 dropdownlist 進行並核取方塊。
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f7723895dd50b1923de49ca4a3a7055bbad5fe4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832653"
---
<a name="customizing-the-datalists-editing-interface-c"></a>自訂 DataList 的編輯介面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe)或[下載 PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> 在本教學課程中，我們將建立更豐富的編輯介面的 DataList，當中包含 dropdownlist 進行並核取方塊。


## <a name="introduction"></a>簡介

標記和 DataList s 中的 Web 控制項`EditItemTemplate`定義其編輯介面。 在所有可編輯的 DataList 範例我們 ve 檢查到目前為止，可編輯的文字方塊中的 Web 控制項編寫介面。 在 [前述教學課程](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)我們改善的編輯階段的使用者體驗將驗證控制項加入。

`EditItemTemplate`可以進一步展開成包含 Web 控制項以外的文字方塊中，例如 dropdownlist 進行，RadioButtonLists，行事曆等等。 如同文字方塊中，自訂以包含其他 Web 控制項的編輯介面時，會採用下列步驟：

1. 將 Web 控制項加入`EditItemTemplate`。
2. 您可以使用資料繫結語法，將對應的資料欄位值指派給適當的屬性。
3. 在 `UpdateCommand`事件處理常式，以程式設計方式存取 Web 控制值，並將它傳遞到適當的 BLL 方法。

在本教學課程中，我們將建立更豐富的編輯介面的 DataList，當中包含 dropdownlist 進行並核取方塊。 特別是，我們將在其中建立資料清單，列出產品資訊，並允許更新 s 產品名稱、 供應商、 類別和已停止的狀態 （請參閱 圖 1）。


[![編輯介面包括文字方塊、 兩個 dropdownlist 進行，並核取方塊](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**圖 1**： 編輯介面包含文字方塊中，兩個 dropdownlist 進行，並核取方塊 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>步驟 1︰ 顯示產品資訊

我們可以建立 DataList s 可編輯介面之前，我們首先要建置的唯寫介面。 首先開啟`CustomizedUI.aspx`頁面上，從`EditDeleteDataList`資料夾並從設計工具中，DataList 加入頁面上，設定其`ID`屬性設`Products`。 從 DataList s 智慧標籤，建立新的 ObjectDataSource。 命名新 ObjectDataSource`ProductsDataSource`並將它設定為從中擷取資料`ProductsBLL`類別的`GetProducts`方法。 為先前可編輯的 DataList 教學課程我們將直接前往商業邏輯層更新已編輯的產品的資訊。 因此，設定下拉式清單中的 UPDATE、 INSERT、，然後刪除為 [（無）] 索引標籤。


[![設定為 （無） 更新、 插入和刪除索引標籤的下拉式清單](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**圖 2**： 將更新、 插入和刪除索引標籤下拉式清單設定為 （無） ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


設定 ObjectDataSource 之後，Visual Studio 會建立預設`ItemTemplate`DataList 列出每個資料欄位的名稱和值傳回。 修改`ItemTemplate`使範本清單中的產品名稱`<h4>`項目和類別名稱、 供應商名稱、 價格和已停止的狀態。 此外，將 [編輯] 按鈕，以確保其`CommandName`屬性設為編輯。 宣告式標記為我`ItemTemplate`遵循：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

產品資訊使用配置上述標記&lt;h4&gt;產品的名稱和四個資料行的標題`<table>`其餘欄位。 `ProductPropertyLabel`並`ProductPropertyValue`中定義的 CSS 類別`Styles.css`，在先前的教學課程中討論過。 圖 3 顯示我們透過瀏覽器檢視時的進度。


[![顯示名稱、 供應商、 類別、 已停止的狀態，以及每個產品的價格](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**圖 3**： 顯示名稱、 供應商、 分類、 已停止的狀態，以及每個產品的價格 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>步驟 2： 將 Web 控制項新增至編輯介面

建置自訂的 DataList 編輯介面是新增至所需的 Web 控制項的第一個步驟`EditItemTemplate`。 特別是，我們需要 dropdownlist 進行分類，另一個用於供應商，並已停止狀態的核取方塊。 由於產品的價格不能編輯在此範例中，我們可以繼續使用 Label Web 控制項顯示。

若要自訂編輯介面，按一下 [編輯範本] 連結，從 DataList s 智慧標籤，然後選擇`EditItemTemplate`從下拉式清單中的選項。 新增至 DropDownList`EditItemTemplate`並將其`ID`至`Categories`。


[![新增 DropDownList 類別](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**圖 4**： 新增 DropDownList 類別 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


接下來，從 DropDownList s 智慧標籤，選取 [選擇資料來源] 選項，並建立名為新 ObjectDataSource `CategoriesDataSource`。 設定要使用這個 ObjectDataSource`CategoriesBLL`類別的`GetCategories()`方法 （請參閱 [圖 5]）。 接下來，DropDownList s 中的資料來源組態精靈會提示輸入要用於每個資料欄位`ListItem`s`Text`和`Value`屬性。 DropDownList 顯示`CategoryName`資料欄位並使用`CategoryID`做為值，如 圖 6 所示。


[![建立名為 CategoriesDataSource 新 ObjectDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**圖 5**： 建立新的 ObjectDataSource 具名`CategoriesDataSource`([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![設定 DropDownList 的顯示，且值欄位](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**圖 6**： 設定 DropDownList 的顯示和值欄位 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


重複這一系列的步驟來建立 DropDownList 供應商。 設定`ID`針對至這個 DropDownList`Suppliers`並命名為其 ObjectDataSource `SuppliersDataSource`。

新增兩個 dropdownlist 進行之後, 加入已停止狀態的核取方塊和文字方塊中，針對產品的名稱。 設定`ID`核取方塊的文字方塊`Discontinued`和`ProductName`分別。 新增 RequiredFieldValidator，以確保使用者，提供產品的名稱的值。

最後，新增 [更新] 和 [取消] 按鈕。 請記住，這兩個按鈕命令式，其`CommandName`屬性會設為 更新 和 取消，分別。

放心地配置您隨心所欲的編輯介面。 我已選擇要使用的相同資料行的四個`<table>`版面配置從唯讀介面，為下列宣告式語法和螢幕擷取畫面說明：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![編輯介面是配置 Out Read-Only 介面](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**圖 7**: 編輯介面是配置 Out Read-Only 介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>步驟 3： 建立 EditCommand 及 CancelCommand 事件處理常式

目前沒有任何資料繫結語法`EditItemTemplate`(除了`UnitPriceLabel`，這逐字從複製`ItemTemplate`)。 我們將新增資料繫結語法會暫時的但第一個讓的 DataList s 建立事件處理常式`EditCommand`和`CancelCommand`事件。 請記得，負責`EditCommand`事件處理常式是要呈現已按下的 [編輯] 按鈕，DataList 項目的編輯介面，而`CancelCommand`的作業是以 DataList 返回其預先編輯的狀態。

建立這些兩個事件處理常式，並讓它們使用下列程式碼：


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

使用這些兩個事件處理常式的位置，按一下 [編輯] 按鈕顯示的編輯介面，並按一下 [取消] 按鈕為唯讀模式中傳回的已編輯的項目。 [圖 8] 顯示 DataList Chef Anton 的 Gumbo 混合的已按下 [編輯] 按鈕之後。 因為我們尚未加入任何資料繫結語法編輯介面，ve`ProductName`文字方塊為空白，`Discontinued`從選取的核取方塊未核取和第一個項目`Categories`和`Suppliers`dropdownlist 進行。


[![按一下 [編輯] 按鈕會顯示編輯介面](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**圖 8**： 按一下 [編輯] 按鈕顯示的編輯介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>步驟 4： 將資料繫結語法新增至編輯介面

若要顯示目前的產品的值的編輯介面，我們需要使用資料繫結語法來將資料欄位值指派給適當的 Web 控制項值。 可以透過設計工具套用資料繫結語法，移至 編輯範本 畫面，並選取 編輯資料繫結連結從 Web 控制項的智慧標籤。 或者，資料繫結語法可以直接加入宣告式標記。

指派`ProductName`資料欄位值為`ProductName`TextBox s`Text`屬性，`CategoryID`並`SupplierID`資料欄位值`Categories`和`Suppliers`dropdownlist 進行`SelectedValue`屬性，和`Discontinued`資料欄位值為`Discontinued`核取方塊的`Checked`屬性。 進行這些變更，透過設計工具，或是直接透過宣告式標記中之後, 重新瀏覽透過瀏覽器頁面，然後按一下 [編輯] 按鈕，Chef Anton s Gumbo 混合。 如 [圖 9] 所示，資料繫結語法已經新增目前的值到 TextBox、 dropdownlist 進行，並核取方塊。


[![按一下 [編輯] 按鈕會顯示編輯介面](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**圖 9**： 按一下 [編輯] 按鈕顯示的編輯介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>步驟 5： 儲存使用者的變更 UpdateCommand 的事件處理常式

當使用者編輯產品，並按一下 [更新] 按鈕，就會發生回傳和 DataList 的`UpdateCommand`引發事件。 在事件處理常式中，我們要讀取的值中的 Web 控制項從`EditItemTemplate`和與 BLL，若要在資料庫中的產品更新的介面。 因為我們在先前的教學課程中看到的 ve`ProductID`更新的產品是可透過存取`DataKeys`集合。 以程式設計方式參考使用的 Web 控制項，即可存取的使用者輸入欄位`FindControl("controlID")`，如下列程式碼所示：


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

程式碼一開始諮詢`Page.IsValid`以確保頁面上的所有驗證控制項都是有效的屬性。 如果`Page.IsValid`已`True`，已編輯的產品 s`ProductID`值從讀取`DataKeys`收集和 Web 控制項中的資料項目`EditItemTemplate`以程式設計方式參考。 接下來，從這些 Web 控制項的值會讀入接著會傳遞至適當的變數`UpdateProduct`多載。 更新資料之後, DataList 會回到其預先編輯的狀態。

> [!NOTE]
> 我已省略的例外狀況處理邏輯中加入[處理 BLL 和 DAL 層級例外狀況](handling-bll-and-dal-level-exceptions-cs.md)信守的程式碼，此範例中的教學課程已取得焦點。 作為練習，請完成本教學課程之後新增這項功能。


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>步驟 6： 處理 NULL CategoryID 和供應商編號值

Northwind 資料庫允許用於`NULL`的值`Products`表格 s`CategoryID`和`SupplierID`資料行。 不過，我們的編輯介面不 t 目前容納`NULL`值。 如果我們嘗試要編輯之產品的`NULL`值是其`CategoryID`或`SupplierID`資料行，我們會得到`ArgumentOutOfRangeException`具有類似的錯誤訊息： *'Categories' 具有無效，因為它 SelectedValue中的項目清單不存在。* 此外，有 s 目前沒有辦法變更產品的分類或供應商值從非`NULL`值`NULL`其中一個。

若要支援`NULL`category 與 supplier dropdownlist 進行的值，我們需要新增其他`ListItem`。 我已選擇使用 （無） 做`Text`值，這個`ListItem`，但您可以將它變更為其他 （例如空字串） 如果您 d。 最後，請記得設定 dropdownlist 進行`AppendDataBoundItems`要`True`; 如果您忘記執行此操作，類別目錄，而且繫結至 DropDownList 供應商將會覆寫以靜態方式新增`ListItem`。

進行這些變更，DataList s 中的 dropdownlist 進行標記之後`EditItemTemplate`看起來應該如下所示：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> 靜態`ListItem`s 可以新增至 DropDownList 透過設計工具，或直接透過宣告式語法。 加入以代表資料庫的 DropDownList 項目時`NULL`值，請務必新增`ListItem`透過宣告式語法。 如果您使用`ListItem`設計工具中的集合編輯器，將會省略產生的宣告式語法`Value`一併設定時指派為空白的字串，建立類似的宣告式標記： `<asp:ListItem>(None)</asp:ListItem>`。 雖然這看起來無害，遺漏`Value`會導致使用 DropDownList`Text`在其位置中的屬性值。 這表示，如果這`NULL``ListItem`已選取，（無） 的值將會嘗試指派給 [product] 資料欄位 (`CategoryID`或`SupplierID`，在本教學課程)，從而導致例外狀況。 藉由明確將`Value=""`，則`NULL`值會指派給產品資料欄位`NULL``ListItem`已選取。


請花一點時間檢閱我們透過瀏覽器的進度。 當您編輯產品，請注意，`Categories`和`Suppliers`dropdownlist 進行這兩個具有 （無） 開頭的 DropDownList 的選項。


[![類別和供應商 dropdownlist 進行包含 （無） 選項](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**[圖 10**:`Categories`並`Suppliers`dropdownlist 進行包含 （無）] 選項 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


儲存 （無） 選項為資料庫`NULL`值，我們需要回到`UpdateCommand`事件處理常式。 變更`categoryIDValue`並`supplierIDValue`是可為 null 的整數，並將其指派的值以外的其他變數`Nothing`才 DropDownList 的`SelectedValue`不是空字串：


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

透過這項變更，值為`Nothing`會傳遞至`UpdateProduct`BLL 方法，如果使用者選取 （無） 選項的下拉式清單中，對應至任一`NULL`資料庫值。

## <a name="summary"></a>總結

在本教學課程中，我們了解如何建立更複雜的 DataList 編輯介面包含三個不同的輸入的 Web 控制項的文字方塊中，兩個 dropdownlist 進行，以及驗證控制項的核取方塊的內容。 建置時編輯介面，步驟也不論所使用的 Web 控制項： 新增至 DataList s 的 Web 控制項開始`EditItemTemplate`; 使用資料繫結語法來指派適當的網頁與對應的資料欄位值控制項屬性;然後在`UpdateCommand`事件處理常式，以程式設計方式存取的 Web 控制項和其適當的屬性，將其值傳遞到 BLL。

時是否建立編輯介面，它只是文字方塊或不同的 Web 控制項的集合組成 s，所以請務必正確處理資料庫`NULL`值。 當考慮`NULL`s，它是命令式的您不只會正確顯示現有`NULL`值中的編輯介面，但您提供一種方式讓值變為`NULL`。 針對在 DataLists dropdownlist 進行，這通常是指新增靜態`ListItem`其`Value`屬性明確設定為空字串 (`Value=""`)，並新增一些程式碼項`UpdateCommand`事件處理常式來判斷是否`NULL``ListItem`已選取。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dennis Patterson、 David Suru 和 Randy Schmidt。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [下一頁](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
