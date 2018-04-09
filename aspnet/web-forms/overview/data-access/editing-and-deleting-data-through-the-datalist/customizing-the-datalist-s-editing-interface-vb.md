---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: 自訂資料清單的編輯介面 (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中我們會建立更豐富的編輯介面 DataList，其中包括 DropDownLists 和核取方塊。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-datalists-editing-interface-vb"></a>自訂資料清單的編輯介面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe)或[下載 PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> 在本教學課程中我們會建立更豐富的編輯介面 DataList，其中包括 DropDownLists 和核取方塊。


## <a name="introduction"></a>簡介

標記和 DataList s 中的 Web 控制項`EditItemTemplate`定義其編輯介面。 中的所有可編輯的 DataList 範例我們 ve 檢查為止，可編輯的文字方塊中的 Web 控制項編寫介面。 在[前述教學課程](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)我們改進藉由新增驗證控制項的編輯階段的使用者體驗。

`EditItemTemplate`可以進一步展開成包含 Web 控制項以外，請在文字方塊中，例如 DropDownLists，RadioButtonLists，行事曆，依此類推。 如同等文字方塊中，自訂會包括其他 Web 控制項的編輯介面時，使用下列步驟：

1. 新增 Web 控制項`EditItemTemplate`。
2. 您可以使用資料繫結語法，將對應的資料欄位值指派給適當的屬性。
3. 在`UpdateCommand`事件處理常式，以程式設計方式存取的 Web 控制項值，並將它傳遞至適當的 BLL 方法。

在本教學課程中我們會建立更豐富的編輯介面 DataList，其中包括 DropDownLists 和核取方塊。 特別是，我們將建立資料清單，列出產品資訊，並允許更新 s 產品名稱、 供應商、 類別和已停止的狀態 （請參閱圖 1）。


[![編輯介面包括文字方塊、 兩個 DropDownLists 和一個核取方塊](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**圖 1**： 編輯介面包括文字方塊、 兩個 DropDownLists 和一個核取方塊 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>步驟 1： 顯示產品資訊

我們可以建立 DataList s 可編輯介面之前，我們需要先建立唯讀介面。 先開啟`CustomizedUI.aspx`頁面`EditDeleteDataList`資料夾並從設計工具中，DataList 加入頁面上，設定其`ID`屬性`Products`。 從 DataList s 智慧標籤，建立新的 ObjectDataSource。 將這個新 ObjectDataSource`ProductsDataSource`並將它設定為從中擷取資料`ProductsBLL`類別的`GetProducts`方法。 做為前一個可編輯 DataList 教學課程中，我們會直接前往商務邏輯層更新編輯的產品的資訊。 因此，設定下拉式清單中更新、 插入和刪除定位點為 （無）。


[![設定為 （無） 更新、 插入和刪除索引標籤的下拉式清單](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**圖 2**： 設定更新、 插入和刪除索引標籤下拉式清單會列出為 （無） ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


設定後 ObjectDataSource，Visual Studio 會建立預設`ItemTemplate`列出每個資料欄位的名稱和值的資料清單傳回。 修改`ItemTemplate`使範本清單中的產品名稱`<h4>`項目和類別名稱、 供應商名稱、 價格和已停止的狀態。 此外，將 [編輯] 按鈕，並確定其`CommandName`屬性設定為 Edit。 宣告式標記我`ItemTemplate`遵循：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

產品資訊使用配置上述標記&lt;h4&gt; s 產品名稱和四個資料行標題`<table>`其餘欄位。 `ProductPropertyLabel`和`ProductPropertyValue`中定義的 CSS 類別`Styles.css`，已經討論過在先前的教學課程。 圖 3 顯示我們透過瀏覽器檢視時的進度。


[![顯示名稱、 供應商、 類別、 已停止的狀態，以及每個產品價格](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**圖 3**: Name、 供應商、 分類、 已停止的狀態，以及每個產品價格會顯示 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>步驟 2： 加入編輯介面的 Web 控制項

建置自訂的 DataList 編輯介面是加入至所需的 Web 控制項的第一個步驟`EditItemTemplate`。 特別是，我們需要 DropDownList 分類中，另一個供應商，並已停止狀態的核取方塊。 因為產品的價格不在此範例中，您可編輯，所以我們可以繼續使用標籤 Web 控制項顯示。

若要自訂編輯介面，請按一下 [編輯樣板] 連結，從 DataList s 智慧標籤，然後選擇`EditItemTemplate`從下拉式清單選項。 加入至 DropDownList`EditItemTemplate`並設定其`ID`至`Categories`。


[![新增的類別目錄的 DropDownList](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**圖 4**: DropDownList 加入類別 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


接下來，從 DropDownList s 智慧標籤，選取 [選擇資料來源] 選項，並建立名為新 ObjectDataSource `CategoriesDataSource`。 設定要使用此 ObjectDataSource`CategoriesBLL`類別的`GetCategories()`方法 （請參閱圖 5）。 接下來，DropDownList 的資料來源組態精靈會提示輸入要用於每個資料欄位`ListItem`s`Text`和`Value`屬性。 DropDownList 顯示`CategoryName`資料欄位和使用`CategoryID`的值，如圖 6 所示。


[![建立名為 CategoriesDataSource 新 ObjectDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**圖 5**： 建立新的 ObjectDataSource 具名`CategoriesDataSource`([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![設定 DropDownList 的顯示，且值欄位](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**圖 6**： 設定 DropDownList 的顯示和值欄位 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


重複這一系列的步驟為建立 DropDownList 供應商。 設定`ID`針對此 DropDownList 以`Suppliers`並命名它 ObjectDataSource `SuppliersDataSource`。

加入兩個 DropDownLists 之後, 加入已停止狀態的核取方塊和產品 s 名稱文字方塊。 設定`ID`核取方塊的文字方塊`Discontinued`和`ProductName`分別。 新增 RequiredFieldValidator 以確保使用者，提供產品的名稱的值。

最後，新增更新 和 取消 5d; 按鈕。 請記住，這兩個按鈕命令式，其`CommandName`屬性設定為 更新 和 取消，分別。

請隨意配置然而您要編輯的介面。 我已選擇使用同一個四欄`<table>`配置從唯讀介面，為下列宣告式語法和螢幕擷取畫面說明：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![編輯介面是配置出像唯讀介面](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**圖 7**: 編輯介面已配置推出像唯讀介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>步驟 3： 建立 EditCommand 和 CancelCommand 事件處理常式

目前沒有在任何資料繫結語法`EditItemTemplate`(除了`UnitPriceLabel`，其已透過複製逐字從`ItemTemplate`)。 我們將會立刻顯示，將新增資料繫結語法，但第一個可讓 s 為 DataList s 建立事件處理常式`EditCommand`和`CancelCommand`事件。 請記得的責任`EditCommand`事件處理常式會呈現已按下的 [編輯] 按鈕，DataList 項目的編輯介面，而`CancelCommand`的作業會傳回在 DataList 成預先編輯狀態。

建立這些兩個事件處理常式，並讓它們使用下列程式碼：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

這些兩個事件處理常式的位置，按一下 編輯 按鈕會顯示編輯介面，並按一下取消 5d; 按鈕傳回已編輯的項目為唯讀模式。 圖 8 顯示在 DataList 之後 Chef Anton 的 Gumbo 混合的已按下 [編輯] 按鈕。 因為我們尚未加入任何資料繫結的語法編輯介面中，我們`ProductName`文字方塊為空白，`Discontinued`核取方塊未選取和第一個項目選取`Categories`和`Suppliers`DropDownLists。


[![按一下 [編輯] 按鈕會顯示編輯介面](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**圖 8**： 按一下 [編輯] 按鈕會顯示在編輯介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>步驟 4： 加入資料繫結語法來編輯介面

若要顯示目前的產品的值編輯介面，我們需要使用資料繫結語法來將資料欄位值指派給適當的 Web 控制項值。 資料繫結語法可以套用透過設計工具，移至 編輯樣板 畫面，並選取 從網站的 編輯資料繫結連結控制項的智慧標籤。 或者，資料繫結語法可以直接加入宣告式標記。

指派`ProductName`資料欄位值，`ProductName`文字方塊 s`Text`屬性，`CategoryID`和`SupplierID`資料欄位值`Categories`和`Suppliers`DropDownLists`SelectedValue`屬性，而`Discontinued`資料欄位值，`Discontinued`核取方塊的`Checked`屬性。 透過設計工具，或是直接透過宣告式標記，這些變更後再次瀏覽透過瀏覽器頁面，按一下 [編輯] 按鈕，Chef Anton s Gumbo 混合。 如圖 9 所示，資料繫結語法已經新增目前的值到文字方塊中、 DropDownLists 和核取方塊。


[![按一下 [編輯] 按鈕會顯示編輯介面](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**圖 9**： 按一下 [編輯] 按鈕會顯示在編輯介面 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>步驟 5： 儲存使用者的變更 UpdateCommand 的事件處理常式

當使用者編輯產品，並按一下 [更新] 按鈕，就會發生回傳，DataList 的`UpdateCommand`事件引發。 在事件處理常式中，我們要讀取的值中的 Web 控制項從`EditItemTemplate`和 BLL 更新產品在資料庫中的使用的介面。 當我們先前的教學課程中所見過`ProductID`更新的產品是可透過存取`DataKeys`集合。 以程式設計方式參考使用的 Web 控制項，即可存取的使用者輸入欄位`FindControl("controlID")`，如下列程式碼所示：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

程式碼一開始會判定`Page.IsValid`以確保頁面上的所有驗證控制項都是有效的屬性。 如果`Page.IsValid`是`True`，編輯的產品 s`ProductID`值從讀取`DataKeys`集合和 Web 控制項中的資料項目`EditItemTemplate`以程式設計方式參考。 接下來，從這些 Web 控制項的值讀入接著會傳遞到適當的變數`UpdateProduct`多載。 之後更新資料，DataList 傳回成預先編輯狀態。

> [!NOTE]
> 我已省略例外狀況處理邏輯中加入[處理 BLL-以及 DAL 層級例外狀況](handling-bll-and-dal-level-exceptions-vb.md)為了讓程式碼，此範例中的教學課程已取得焦點。 做為練習，請完成本教學課程之後加入這項功能。


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>步驟 6： 處理 NULL CategoryID 和供應商編號值

Northwind 資料庫允許用於`NULL`值`Products`資料表 s`CategoryID`和`SupplierID`資料行。 不過，我們編輯介面規定 t 目前容納`NULL`值。 如果我們嘗試編輯產品的`NULL`值是其`CategoryID`或`SupplierID`資料行，就會得到`ArgumentOutOfRangeException`與類似的錯誤訊息： *'類別' 具有無效，因為它進行的 SelectedValue項目清單中不存在。* 此外，有 s 目前沒有方法可變更 s 產品類別目錄或供應商值從非`NULL`值設定為`NULL`其中一個。

若要支援`NULL`類別目錄和供應商 DropDownLists 的值，我們需要加入其他`ListItem`。 我已選擇要做為 （無）`Text`值，這個`ListItem`，但您可以將它變更為東西 （例如空字串） 如您 d。 最後，請記得設定 DropDownLists`AppendDataBoundItems`至`True`; 如果您忘了這麼做，類別目錄，而且繫結至 DropDownList 供應商將會覆寫以靜態方式新增`ListItem`。

進行這些變更，在 DataList 的 DropDownLists 標記後`EditItemTemplate`看起來應該類似下列：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> 靜態`ListItem`s 可以加入 DropDownList 透過設計工具，或直接透過宣告式語法。 當加入 DropDownList 項目，表示資料庫`NULL`值，請務必新增`ListItem`透過宣告式語法。 如果您使用`ListItem`集合編輯器中設計工具中，產生的宣告式語法將會省略`Value`完全設定時指派的空白字串，建立類似的宣告式標記： `<asp:ListItem>(None)</asp:ListItem>`。 這看起來無害的而遺漏`Value`會導致使用 DropDownList`Text`其所在位置的屬性值。 這表示，如果這`NULL``ListItem`是選取的值 （無） 會嘗試指派給產品資料欄位 (`CategoryID`或`SupplierID`，在本教學課程)，這會造成例外狀況。 藉由明確地設定`Value=""`、`NULL`值會指派給產品資料欄位時`NULL``ListItem`已選取。


請花一點時間來檢視進度透過瀏覽器。 當編輯產品，請注意，`Categories`和`Suppliers`DropDownLists 這兩個具有 （無） 開頭的 DropDownList 的選項。


[![分類和供應商 DropDownLists 包含 （無） 選項](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**圖 10**:`Categories`和`Suppliers`DropDownLists 包含 （無） 選項 ([按一下以檢視完整大小的影像](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


若要儲存 （無） 選項做為資料庫`NULL`值，我們需要回到`UpdateCommand`事件處理常式。 變更`categoryIDValue`和`supplierIDValue`是可為 null 整數並指派值以外的變數`Nothing`才 DropDownList 的`SelectedValue`不為空字串：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

透過這項變更，值為`Nothing`會傳遞至`UpdateProduct`BLL 方法，如果使用者選取 （無） 選項的下拉式清單中，對應至任一`NULL`資料庫值。

## <a name="summary"></a>總結

在此教學課程中我們可了解如何建立更複雜的 DataList 編輯介面，其中包含三個不同輸入的 Web 控制項文字方塊中，兩個 DropDownLists，以及驗證控制項的核取方塊。 建置時編輯介面，會採用相同步驟不論所使用的 Web 控制項： 將 Web 控制項加入至 DataList s 啟動`EditItemTemplate`; 若要指派適當的 Web 與對應的資料欄位值中使用資料繫結語法控制項屬性。然後在`UpdateCommand`事件處理常式，以程式設計方式存取的 Web 控制項和適當屬性，將其值傳遞到 BLL。

時是否建立編輯介面，它只是文字方塊或不同的 Web 控制項的集合組成 s，所以請務必正確地處理資料庫`NULL`值。 當帳戶處理`NULL`s，務必在您不只會正確顯示現有`NULL`值編輯介面，但您提供一種方法讓值變為`NULL`。 如 DropDownLists DataLists 中，這通常表示新增靜態`ListItem`其`Value`屬性明確設定為空字串 (`Value=""`)，並加入的程式碼，以位元`UpdateCommand`事件處理常式，以判斷是否`NULL``ListItem`已選取。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Dennis Patterson、 David Suru 和袁 Schmidt。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
