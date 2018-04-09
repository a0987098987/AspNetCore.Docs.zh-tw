---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: 自訂資料修改介面 (C#) |Microsoft 文件
author: rick-anderson
description: 在此教學課程中我們會探討如何以自訂的可編輯的 GridView 中介面取代標準的文字方塊和核取方塊會控制與 alternati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f25b265c50870d59721a94c01d78f589a5d5f1c3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-data-modification-interface-c"></a>自訂資料修改介面 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe)或[下載 PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> 在此教學課程中我們會探討如何以自訂的可編輯的 GridView 中介面取代標準的文字方塊和核取方塊控制項替代輸入 Web 控制項。


## <a name="introduction"></a>簡介

BoundFields 和 CheckBoxFields GridView 和 DetailsView 控制項所使用的簡化程序修改資料，因為其呈現唯讀、 可編輯的與可插入介面的能力。 可以轉譯這些介面，而不需要新增任何額外的宣告式標記或程式碼。 不過，BoundField 和 CheckBoxField 的介面缺乏真實世界案例中通常需要自訂。 若要自訂 GridView 或 DetailsView 中的可編輯或可插入介面我們要改用 TemplateField。

在[前述教學課程](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)我們可了解如何加入驗證的 Web 控制項來自訂資料修改介面。 在此教學課程中我們將探討如何自訂實際的資料集合 Web 控制項，取代 BoundField 和 CheckBoxField 的標準的文字方塊和核取方塊控制項，與替代輸入的 Web 控制項。 特別是，我們會建置可讓產品名稱、 類別、 供應商和已停止的狀態更新可以讓您編輯 GridView。 當編輯特定的資料列時，[類別目錄和供應商] 欄位會轉譯為 DropDownLists，包含一組可用的類別和可從中選擇的供應商。 此外，我們將會取代將 CheckBoxField 預設核取方塊為 RadioButtonList 控制項提供兩個選項: 「 作用中 」 和 「 停止 」。


[![在 GridView 的編輯介面包括 DropDownLists 和選項按鈕](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**圖 1**: GridView 編輯介面包含 DropDownLists 和選項按鈕 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>步驟 1： 建立適當`UpdateProduct`多載

在本教學課程中，我們將建置可編輯的 GridView 會允許編輯產品的名稱、 類別、 供應商，而且停用狀態。 因此，我們需要`UpdateProduct`接受五個下列四個產品值輸入參數的多載加上`ProductID`。 如同我們先前的多載，這一個將會：

1. 從指定的資料庫擷取產品資訊`ProductID`，
2. 更新`ProductName`， `CategoryID`， `SupplierID`，和`Discontinued`欄位，以及
3. 更新要求傳送到透過 TableAdapter 的 DAL`Update()`方法。

為求簡潔，針對這個特定的多載我已省略商務規則檢查，以確保標示為已停止的產品不是唯一產品的供應商所提供的產品。 如果您想要的話，將它加入逕或者，理想情況下，重構出另一個方法的邏輯。

下列程式碼顯示新`UpdateProduct`中多載`ProductsBLL`類別：


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>步驟 2： 製作可編輯的 GridView

與`UpdateProduct`加入多載，我們就可以準備建立我們可以讓您編輯 GridView。 開啟`CustomizedUI.aspx`頁面`EditInsertDelete`資料夾並將 GridView 控制項加入至設計工具。 接下來，建立新的 ObjectDataSource 從 GridView 的智慧標籤。 設定以擷取產品資訊透過 ObjectDataSource`ProductBLL`類別的`GetProducts()`方法，以及更新產品資料使用`UpdateProduct`剛才所建立的多載。 INSERT 和 DELETE] 索引標籤上，選取 [從下拉式清單 （無）。


[![設定要使用剛才建立的 UpdateProduct 多載 ObjectDataSource](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**圖 2**： 設定要使用 ObjectDataSource`UpdateProduct`多載剛建立 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image6.png))


如我們所見整個資料修改教學課程中，會將指派由 Visual Studio 建立 ObjectDataSource 的宣告式語法`OldValuesParameterFormatString`屬性`original_{0}`。 這當然，不適用於具有我們商務邏輯層因為我們方法不需要原始`ProductID`傳入的值。 因此，如我們所做過先前的教學課程中，請花一點時間指派此屬性移除宣告式語法，或請將這個屬性的值設定為`{0}`。

此變更後，ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

請注意，`OldValuesParameterFormatString`屬性已經移除，而且沒有`Parameter`中`UpdateParameters`所預期的輸入參數的每個集合我們`UpdateProduct`多載。

在 GridView ObjectDataSource 更新的產品值子集設定時，目前顯示*所有*的產品欄位。 請花一點時間編輯 GridView 以便：

- 它只包括`ProductName`， `SupplierName`， `CategoryName` BoundFields 和`Discontinued`CheckBoxField
- `CategoryName`和`SupplierName`看起來像之前 （左邊） 欄位`Discontinued`CheckBoxField
- `CategoryName`和`SupplierName`BoundFields'`HeaderText`屬性分別設定為 「 類別目錄 」 和 「 供應商 」
- 啟用編輯支援 （請檢查 GridView 的智慧標籤的 啟用編輯核取方塊）

這些變更之後，請在設計工具看起來會類似於圖 3，如下所示的 GridView 的宣告式語法。


[![從 GridView 移除不必要的欄位](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**圖 3**： 從 GridView 移除不必要的欄位 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

此時 GridView 的唯讀行為已完成。 檢視資料時，每項產品會轉譯為在 GridView 中顯示產品的名稱、 類別、 供應商的資料列，而且停用狀態。


[![在 GridView 的唯讀介面已完成](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**圖 4**: GridView 唯讀介面已完成 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> 中所述[概觀的插入、 更新和刪除資料的教學課程](an-overview-of-inserting-updating-and-deleting-data-cs.md)，是非常重要的 GridView 的檢視狀態是啟用 （預設行為）。 如果您將 GridView s`EnableViewState`屬性`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行問題與 ASP.NET 2.0 GridViews/DetailsView/FormViews 該支援編輯和/或刪除與的檢視狀態](http://scottonwriting.net/sowblog/posts/10054.aspx)如需詳細資訊。


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>步驟 3： 使用類別和編輯介面的供應商的 DropDownList

請記得，`ProductsRow`物件包含`CategoryID`， `CategoryName`， `SupplierID`，和`SupplierName`屬性，提供在實際的外部索引鍵識別碼值`Products`資料庫資料表和對應`Name`中的值`Categories`和`Suppliers`資料表。 `ProductRow`的`CategoryID`和`SupplierID`可以同時是讀取和寫入，而`CategoryName`和`SupplierName`屬性會標示為唯讀。

由於的唯讀狀態`CategoryName`和`SupplierName`屬性，對應 BoundFields 有其`ReadOnly`屬性設定為`true`，防止在編輯的資料列時修改這些值。 雖然我們可以設定`ReadOnly`屬性`false`、 呈現`CategoryName`和`SupplierName`BoundFields 為期間編輯的文字方塊，這種方法會導致例外狀況時使用者嘗試更新的產品，因為沒有任何`UpdateProduct`中採用多載`CategoryName`和`SupplierName`輸入。 事實上，我們不想要建立這類多載兩個原因：

- `Products`資料表沒有`SupplierName`或`CategoryName`欄位，但`SupplierID`和`CategoryID`。 因此，我們想要傳遞這些特定識別碼值，不是其查閱資料表值方法。
- 需要使用者輸入供應商或類別目錄的名稱是不盡理想，因為它需要使用者知道可用的類別和供應商和正確的拼字。

供應商] 和 [類別目錄欄位應該會顯示類別目錄和供應商名稱，在唯讀模式中的 （如同現在） 和適用的選項，當正在編輯的下拉式清單。 使用下拉式清單，使用者可以快速查看哪些類別和供應商可供選擇，可以更輕鬆地選取範圍。

若要提供這種行為，我們需要將轉換`SupplierName`和`CategoryName`BoundFields TemplateFields 到其`ItemTemplate`發出`SupplierName`和`CategoryName`值和其`EditItemTemplate`使用 DropDownList 控制項至清單可用的類別和供應商。

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>加入`Categories`和`Suppliers`DropDownLists

啟動轉換`SupplierName`和`CategoryName`到由 TemplateFields BoundFields： 按一下編輯資料行連結從 GridView 的智慧標籤; 選取 BoundField 從左下方; 中的清單，然後按一下 「 轉換成此欄位TemplateField 」 連結。 轉換程序將會同時建立為 TemplateField`ItemTemplate`和`EditItemTemplate`，如下列宣告式語法中所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

因為 BoundField 已標示為唯讀，同時`ItemTemplate`和`EditItemTemplate`包含標籤 Web 控制項`Text`屬性繫結至適用的資料欄位 (`CategoryName`，上述語法中)。 我們需要修改`EditItemTemplate`，取代 DropDownList 控制項標籤 Web 控制項。

如同我們在先前的教學課程中，透過設計工具，或直接從宣告式語法，可以進行編輯的範本。 若要透過設計工具中編輯它，按一下 從 GridView 的智慧標籤的 編輯樣板 連結，並選擇使用 類別 欄位`EditItemTemplate`。 移除標籤 Web 控制項並將它取代 DropDownList 控制項，DropDownList 的 ID 屬性設定為`Categories`。


[![移除 文字方塊中，並加入 EditItemTemplate 的 DropDownList](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**圖 5**： 移除 文字方塊中，並加入至 DropDownList `EditItemTemplate` ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image15.png))


接下來，我們需要填入可用的類別與 DropDownList。 按一下 選擇資料來源連結，從 DropDownList 的智慧標籤上，選擇建立名為新 ObjectDataSource `CategoriesDataSource`。


[![建立名為 CategoriesDataSource 新 ObjectDataSource 控制項](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**圖 6**： 建立新的 ObjectDataSource 控制項具名`CategoriesDataSource`([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image18.png))


若要讓此 ObjectDataSource 傳回所有的類別，將它繫結`CategoriesBLL`類別的`GetCategories()`方法。


[![ObjectDataSource，繫結 CategoriesBLL GetCategories() 方法](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**圖 7**： 繫結至 ObjectDataSource`CategoriesBLL`的`GetCategories()`方法 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image21.png))


最後，設定 DropDownList 使得`CategoryName`欄位會顯示在每個 DropDownList`ListItem`與`CategoryID`做為值的欄位。


[![有顯示 [類別名稱] 欄位和 CategoryID 做為值](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**圖 8**： 有`CategoryName`顯示欄位而`CategoryID`做為值 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image24.png))


進行這些變更的宣告式標記之後`EditItemTemplate`中`CategoryName`TemplateField 將包含 DropDownList 和 ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> 在 DropDownList`EditItemTemplate`必須啟用其檢視狀態。 我們很快就會加入資料繫結語法 DropDownList 的宣告式語法，例如資料繫結命令`Eval()`和`Bind()`只能出現在已啟用其檢視狀態的控制項。


重複這些步驟，加入名為 DropDownList`Suppliers`至`SupplierName`TemplateField 的`EditItemTemplate`。 這會牽涉到新增 DropDownList 以`EditItemTemplate`和建立另一個 ObjectDataSource。 `Suppliers` DropDownList 的 ObjectDataSource，不過，應該設定為叫用`SuppliersBLL`類別的`GetSuppliers()`方法。 此外，設定`Suppliers`DropDownList 以顯示`CompanyName`欄位，並使用`SupplierID`欄位的值作為其`ListItem`s。

加入兩個 DropDownLists 之後`EditItemTemplate`s，載入瀏覽器中的，然後按一下 [編輯] 按鈕的 Chef Anton 印地安 Seasoning 產品。 如圖 9 所示，產品類別目錄和供應商資料行會轉譯為包含可用的類別和供應商可從中選擇的下拉式清單中。 但請注意，*第一個*這兩個下拉式清單中的項目預設會選取 （飲料類別目錄） 和山為供應商，即使 Chef Anton 印地安 Seasoning 提供新增奧良印地安 Condiment歡樂。


[![預設會選取下拉式清單中第一個項目](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**圖 9**： 預設會選取下拉式清單中的第一個項目 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image27.png))


此外，如果您按一下 [更新] 時，您會發現產品的`CategoryID`和`SupplierID`值都會設為`NULL`。 這兩種討厭的行為會因為在 DropDownLists `EditItemTemplate` s 未繫結到任何資料欄位的基礎產品資料中。

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>繫結至 DropDownLists`CategoryID`和`SupplierID`資料欄位

以具有編輯過的產品類別目錄和供應商下拉式清單設定為適當的值，以及具有這些值傳回到 BLL`UpdateProduct`按一下更新後的方法，我們需要繫結 DropDownLists'`SelectedValue`屬性`CategoryID`和`SupplierID`使用雙向資料繫結的資料欄位。 若要完成此使用`Categories`DropDownList，您可以加入`SelectedValue='<%# Bind("CategoryID") %>'`直接與宣告式語法。

或者，您可以編輯的範本，透過設計工具，然後按一下 編輯資料繫結中的連結 DropDownList 的智慧標籤設定 DropDownList 資料繫結。 接下來，表示`SelectedValue`屬性應該繫結至`CategoryID`欄位使用雙向資料繫結 （請參閱圖 10）。 重複繫結宣告式或設計工具處理`SupplierID`資料欄位至`Suppliers`DropDownList。


[![繫結使用雙向資料繫結的 DropDownList SelectedValue 屬性 CategoryID](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**圖 10**： 繫結`CategoryID`DropDownList 以`SelectedValue`屬性使用雙向資料繫結 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image30.png))


若要套用繫結之後`SelectedValue`的兩個 DropDownLists 內容中，編輯過的產品類別目錄和供應商資料行預設為目前的產品值。 按一下 更新後`CategoryID`和`SupplierID`下拉式清單中選取的項目的值會傳遞至`UpdateProduct`方法。 圖 11 顯示本教學課程之後已加入資料繫結陳述式。請注意如何 Chef Anton 印地安 Seasoning 的下拉式清單中選取項目都正確 Condiment 和新增奧良印地安歡樂。


[![依預設會選取編輯的產品目前類別和供應商值](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**圖 11**： 預設會選取編輯產品的目前類別和供應商值 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>處理`NULL`值

`CategoryID`和`SupplierID`中的資料行`Products`資料表可以是`NULL`，但在 DropDownLists`EditItemTemplate`不包含清單項目來代表`NULL`值。 這有兩種結果：

- 使用者無法使用我們的介面來變更產品類別目錄或供應商，從非`NULL`值設定為`NULL`其中一個
- 如果產品有`NULL``CategoryID`或`SupplierID`，按一下 [編輯] 按鈕時，會導致例外狀況。 這是因為`NULL`所傳回的值`CategoryID`(或`SupplierID`) 中`Bind()`陳述式不會對應到 DropDownList 中的值 (DropDownList 擲回例外狀況時其`SelectedValue`屬性設定為值*不*其集合中的清單項目)。

為了支援`NULL``CategoryID`和`SupplierID`我們需要加入另一個值，`ListItem`來代表每個 DropDownList`NULL`值。 在[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程，我們可了解如何新增其他`ListItem`來資料繫結 DropDownList，這涉及設定 DropDownList`AppendDataBoundItems`屬性`true`和手動加入 其他`ListItem`。 在先前的教學課程中，不過，我們加入了`ListItem`與`Value`的`-1`。 資料繫結中的邏輯在 ASP.NET 中，不過，會自動將轉換的空白字串`NULL`值和相反的。 因此，本教學課程中我們想`ListItem`的`Value`是空字串。

藉由設定這兩個 DropDownLists' 開始`AppendDataBoundItems`屬性`true`。 接下來，加入`NULL``ListItem`中加入下列`<asp:ListItem>`DropDownList 每個項目，讓宣告式標記看起來像：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

我已選擇使用 [（無）] 做為文字值這個`ListItem`，但您可以將它也可以是空白字串，如果您想要變更。

> [!NOTE]
> 中*主要/詳細資料篩選與 DropDownList*教學課程中， `ListItem` s 可以加入至透過設計工具的 DropDownList haciendo 的 DropDownList`Items`屬性 視窗中的屬性 （其中將會顯示`ListItem`集合編輯器)。 不過，請務必新增`NULL``ListItem`本教學課程中透過宣告式語法。 如果您使用`ListItem`集合編輯器 中，產生的宣告式語法將會省略`Value`完全設定時指派的空白字串，建立類似的宣告式標記： `<asp:ListItem>(None)</asp:ListItem>`。 雖然這看起來無害，遺漏值就會使用 DropDownList`Text`其所在位置的屬性值。 這表示，如果這`NULL``ListItem`已選取，"(None) 」 的值將會嘗試指派至`CategoryID`，這會造成例外狀況。 藉由明確地設定`Value=""`、`NULL`值會指派給`CategoryID`時`NULL``ListItem`已選取。


針對供應商 DropDownList 重複這些步驟。

與這個額外`ListItem`，編輯介面現在可以將指派`NULL`值的產品`CategoryID`和`SupplierID`欄位，圖 12 中所示。


[![選擇 （無） 來指派 NULL 值的產品類別目錄或供應商](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**圖 12**: （無） 選擇要指派`NULL`值的產品類別目錄或供應商 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>步驟 4： 使用選項按鈕已停止的狀態

目前產品的`Discontinued`使用 CheckBoxField，轉譯的唯讀資料列的已停用核取方塊與正在編輯的資料列已啟用 核取方塊來表示資料欄位。 通常適合此使用者介面時，我們可以視需要使用 TemplateField 加以自訂。 此教學課程中，我們將變更將 CheckBoxField RadioButtonList 控制項使用兩個選項 「 作用中 」 和 「 停止 」 的使用者可以指定產品的 TemplateField`Discontinued`值。

藉由轉換開始`Discontinued`到 TemplateField，這將會建立具有為 TemplateField CheckBoxField`ItemTemplate`和`EditItemTemplate`。 這兩個範本包含具有核取方塊其`Checked`屬性繫結至`Discontinued`資料欄位中，為，兩者之間唯一的差別`ItemTemplate`的核取方塊的`Enabled`屬性設定為`false`。

取代中的核取方塊`ItemTemplate`和`EditItemTemplate`RadioButtonList 控制項，以設定這兩個 RadioButtonLists'`ID`屬性`DiscontinuedChoice`。 接下來，表示其中 RadioButtonLists 應該每個包含兩個選項按鈕，一個標記為 「 作用中 」 值為"False"並標示為 「 已停止 」 其值為"True"。 若要完成此您可以輸入`<asp:ListItem>`直接透過宣告式語法或使用中的項目`ListItem`從設計工具的集合編輯器。 圖 13`ListItem`已指定兩個選項按鈕選項之後，集合編輯器。


[![Add](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**圖 13**: 「 作用中 」 和 「 Discontinued 」 選項加入 RadioButtonList ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image39.png))


自中 RadioButtonList`ItemTemplate`不應該是可編輯，設定其`Enabled`屬性`false`、 讓`Enabled`屬性`true`（預設值） 中 RadioButtonList 的`EditItemTemplate`。 這會讓選項按鈕，以唯讀狀態，非編輯資料列中，但會允許使用者變更編輯資料列的 RadioButton 值。

我們仍然需要指派的 RadioButtonList 控制項`SelectedValue`屬性選取適當的選項按鈕，以便根據產品的`Discontinued`資料欄位。 如同稍早在本教學課程中檢查 DropDownLists，此資料繫結語法，會加入到宣告式標記的直接或透過編輯資料繫結中的連結 RadioButtonLists 的智慧標籤。

加入兩個 RadioButtonLists 和設定它們之後, `Discontinued` TemplateField 的宣告式標記應該看起來：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

這些變更，`Discontinued`資料行具有不同的核取方塊清單轉換 （請參閱圖 14） 的選項按鈕配對的清單。 當編輯產品時，選取適當的選項按鈕並選取其他選項按鈕，然後按一下 更新進行更新的產品已停止的狀態。


[![選項按鈕組皆已被取代的已停止的核取方塊](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**圖 14**: 停用核取方塊皆已被取代的選項按鈕組 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> 因為`Discontinued`中的資料行`Products`資料庫不能有`NULL`值，我們不需要擔心擷取`NULL`介面中的資訊。 如果，不過，`Discontinued`資料行可能包含`NULL`我們想要新增第三個值 選項按鈕來填入清單其`Value`設為空字串 (`Value=""`)，就像類別以及供應商 DropDownLists。


## <a name="summary"></a>總結

BoundField 和 CheckBoxField 自動呈現唯讀、 編輯及插入介面，而他們缺乏可自訂的能力。 通常，我們需要自訂的編輯，或插入介面，可能將驗證控制項加入 （如同我們在先前的教學課程中所見） 或自訂資料收集的使用者介面 （如我們在本教學課程中所見）。 自訂具有為 TemplateField 介面可以簡化在下列步驟：

1. 新增為 TemplateField 或現有 BoundField 或 CheckBoxField 轉換為 TemplateField
2. 視需要擴充介面
3. 將適當的資料欄位繫結至新加入的 Web 控制項使用雙向資料繫結

除了使用內建 ASP.NET Web 控制項，您也可以自訂自訂、 已編譯的伺服器控制項和使用者控制項具有為 TemplateField 的範本。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [下一頁](implementing-optimistic-concurrency-cs.md)
