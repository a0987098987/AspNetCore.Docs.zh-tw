---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: 自訂資料修改介面 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程將探討如何以自訂的可編輯的 GridView 介面取代標準的文字方塊和核取方塊控制項 alternati...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e04d9c09d9c359c223c70b11ab0f7909182bb46
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808738"
---
<a name="customizing-the-data-modification-interface-c"></a>自訂資料修改介面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe)或[下載 PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> 在本教學課程中我們將探討如何以自訂的可編輯的 GridView 介面取代 TextBox 和 CheckBox 控制項有替代的輸入 Web 控制項。


## <a name="introduction"></a>簡介

BoundFields 和 CheckBoxFields GridView 和 DetailsView 控制項所使用的簡化程序修改資料，因為其呈現唯讀、 可供編輯，以及可插入介面的能力。 可以轉譯這些介面，而不需要新增任何額外的宣告式標記或程式碼。 不過，BoundField 及其的介面會缺少通常需要在真實世界案例中的自訂功能。 若要自訂的可編輯或可插入的介面，在 GridView 或 DetailsView 中我們要改為使用 TemplateField。

在 [前述教學課程](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)我們了解如何藉由將驗證 Web 控制項加入自訂資料修改介面。 在本教學課程中我們將探討如何自訂實際的資料集合 Web 控制項，取代 BoundField 和 CheckBoxField 的 TextBox 和 CheckBox 控制項有替代的輸入 Web 控制項。 特別是，我們將建置可編輯的 GridView，可讓產品名稱、 類別、 供應商和已停止的狀態更新。 當您編輯特定的資料列，category 與 supplier 欄位會轉譯為 dropdownlist 進行，其中包含一組可用的類別和供應商可從中選擇。 此外，我們會將取代 CheckBoxField 預設核取方塊 RadioButtonList 控制項，可提供兩個選項: 「 作用中 」 和 「 已停止 」。


[![GridView 的編輯介面包含 dropdownlist 進行和 Radiobutton](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**圖 1**: GridView 的編輯介面包含 dropdownlist 進行和選項按鈕 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>步驟 1： 建立適當`UpdateProduct`多載

在本教學課程中，我們將建置可編輯的 GridView，允許編輯的產品名稱、 類別、 供應商，並已停止的狀態。 因此，我們需要`UpdateProduct`多載，接受五個輸入參數這些四個產品值加上`ProductID`。 我們先前的多載，用來將此一所示：

1. 從指定的資料庫擷取產品資訊`ProductID`，
2. 更新`ProductName`， `CategoryID`， `SupplierID`，和`Discontinued`欄位，以及
3. 將更新要求傳送至 TableAdapter 的透過 DAL`Update()`方法。

為了簡潔起見，這個特定的多載而言我也省略了商務規則檢查，以確保被標示為已停止的產品不是產品的供應商所提供的唯一產品。 自行將它加入，如果您偏好，或者，理想的情況下，重構出另一個方法的邏輯。

下列程式碼顯示新`UpdateProduct`多載中`ProductsBLL`類別：


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>步驟 2： 製作可編輯的 GridView

使用`UpdateProduct`新增多載，我們已經準備好建立我們可編輯的 GridView。 開啟`CustomizedUI.aspx`頁面中`EditInsertDelete`資料夾並將 GridView 控制項新增至設計工具。 接下來，建立新的 ObjectDataSource 從 GridView 的智慧標籤。 設定要擷取產品資訊透過 ObjectDataSource`ProductBLL`類別的`GetProducts()`方法，並更新產品資料使用`UpdateProduct`我們剛剛建立的多載。 INSERT 和 DELETE] 索引標籤中，選取 [從下拉式清單 （無）。


[![設定要使用剛才建立的 UpdateProduct 多載 ObjectDataSource](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**圖 2**： 設定要使用 ObjectDataSource`UpdateProduct`多載只會建立 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image6.png))


如我們所見整個資料修改教學課程中，會將指派 Visual Studio 所建立的 ObjectDataSource 的宣告式語法`OldValuesParameterFormatString`屬性設`original_{0}`。 這當然不會使用我們的商務邏輯層因為我們的方法不會預期原始`ProductID`必須傳入的值。 因此，在先前的教學課程中，我們已完成之後，請花一點時間若要移除的宣告式語法中的此屬性指派或，相反地，將這個屬性的值設定為`{0}`。

這項變更後 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

請注意，`OldValuesParameterFormatString`已經移除屬性，而且沒有`Parameter`中`UpdateParameters`針對每個所需的輸入參數的集合我們`UpdateProduct`多載。

GridView ObjectDataSource 會設定為更新的產品值子集，而目前顯示*所有*的產品欄位。 請花一點時間，若要編輯的 GridView 以便：

- 它只會包括`ProductName`， `SupplierName`， `CategoryName` BoundFields 和`Discontinued`CheckBoxField
- `CategoryName`並`SupplierName`出現之前 （左邊的） 的欄位`Discontinued`CheckBoxField
- `CategoryName`並`SupplierName`BoundFields'`HeaderText`屬性分別設定為"Category"和"供應商 」
- 編輯支援已啟用 （檢查的 GridView 的智慧標籤的 啟用編輯核取方塊）

這些變更之後，請設計工具會看起來像圖 3 使用 GridView 的宣告式語法如下所示。


[![從 GridView 中移除不必要的欄位](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**圖 3**： 從 GridView 中移除不必要的欄位 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

此時 GridView 的唯讀行為已完成。 檢視資料時，每個產品會轉譯為 GridView，顯示產品名稱、 類別、 供應商中的資料列，並已停止的狀態。


[![GridView 的唯讀介面已完成](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**圖 4**: GridView 的唯讀介面是完成 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> 中所述[概觀的插入、 更新和刪除資料的教學課程](an-overview-of-inserting-updating-and-deleting-data-cs.md)，是非常重要的 GridView 的檢視狀態是啟用 （預設行為）。 如果您將設定 GridView s`EnableViewState`屬性設`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行處理問題與 ASP.NET 2.0 Gridview/DetailsView/FormViews 該支援編輯和/或刪除與的 View State](http://scottonwriting.net/sowblog/posts/10054.aspx)如需詳細資訊。


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>步驟 3： 使用 DropDownList，針對類別目錄和供應商的編輯介面

請記得，`ProductsRow`物件包含`CategoryID`， `CategoryName`， `SupplierID`，並`SupplierName`屬性，可提供實際的外部索引鍵識別碼值，在`Products`資料庫資料表和對應`Name`中的值`Categories`和`Suppliers`資料表。 `ProductRow`的`CategoryID`並`SupplierID`可以兩者都是讀取和寫入，而`CategoryName`和`SupplierName`屬性會標示為唯讀。

由於的唯讀狀態`CategoryName`並`SupplierName`屬性，對應 BoundFields 有其`ReadOnly`屬性設定為`true`，使這些值，無法編輯資料列時修改。 雖然我們可以設定`ReadOnly`屬性，以`false`、 呈現`CategoryName`和`SupplierName`BoundFields 都是在編輯期間的文字方塊，這種方式會導致例外狀況時，使用者嘗試將產品更新，因為沒有任何`UpdateProduct`多載，接受`CategoryName`和`SupplierName`輸入。 事實上，我們不想建立這類多載的原因有二：

- `Products`資料表沒有`SupplierName`或是`CategoryName`欄位，但`SupplierID`和`CategoryID`。 因此，我們想要我們要傳遞這些特定識別碼值，不是其查閱資料表值的方法。
- 要求使用者輸入的供應商或類別目錄的名稱是不盡理想，因為它需要使用者知道可用的類別和供應商和正確的拼字。

供應商] 和 [類別目錄欄位應該會顯示類別目錄和供應商名稱，在唯讀模式中的 （如同現在） 和下拉式清單的編輯時適用的選項。 使用下拉式清單，使用者可以快速查看有哪些類別和供應商可從中選擇，並可以更輕鬆地進行其選取項目。

要提供這種行為，我們需要將轉換`SupplierName`和`CategoryName`BoundFields TemplateFields 到其`ItemTemplate`發出`SupplierName`並`CategoryName`值，且其`EditItemTemplate`使用 DropDownList 控制項至清單可用的類別和供應商。

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>新增`Categories`和`Suppliers`dropdownlist 進行

啟動轉換`SupplierName`和`CategoryName`到由 TemplateFields BoundFields： 按一下從 GridView 的智慧標籤的 [編輯資料行] 連結; 從左下方; 清單中選取 BoundField 和按一下"轉換成此欄位TemplateField 」 連結。 轉換程序會同時建立為 TemplateField`ItemTemplate`和`EditItemTemplate`，如下列宣告式語法中所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

因為 BoundField 已標示為唯讀，同時`ItemTemplate`並`EditItemTemplate`包含 Label Web 控制項`Text`屬性繫結至適用的資料欄位 (`CategoryName`，上述語法中)。 我們需要修改`EditItemTemplate`，DropDownList 控制項以取代 Label Web 控制項。

如我們在先前的教學課程中所見，透過設計工具，或直接從宣告式語法，可以進行編輯的範本。 若要透過設計工具中編輯它，按一下 [編輯範本] 連結，從 GridView 的智慧標籤，並選擇使用 [類別] 欄位`EditItemTemplate`。 移除標籤 Web 控制項，並取代 DropDownList 控制項，將 DropDownList 的 ID 屬性設定為`Categories`。


[![移除 [] 文字方塊中，並加入 EditItemTemplate 的 DropDownList](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**圖 5**： 移除 [] 文字方塊中，並新增至 DropDownList `EditItemTemplate` ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image15.png))


接下來，我們需要填入可用的類別使用 DropDownList。 按一下從 DropDownList 的智慧標籤的 [選擇資料來源] 連結，然後選擇建立新的 ObjectDataSource 名為`CategoriesDataSource`。


[![建立新的 ObjectDataSource 控制項，名為 CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**圖 6**： 建立新的 ObjectDataSource 控制項具名`CategoriesDataSource`([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image18.png))


若要讓這個 ObjectDataSource 傳回所有的類別，將它繫結`CategoriesBLL`類別的`GetCategories()`方法。


[![繫結至 CategoriesBLL GetCategories() 方法的 ObjectDataSource](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**圖 7**： 繫結到 ObjectDataSource`CategoriesBLL`的`GetCategories()`方法 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image21.png))


最後，設定 DropDownList 的設定使得`CategoryName`欄位會顯示在每個 DropDownList`ListItem`與`CategoryID`做為值的欄位。


[![已顯示 [類別名稱] 欄位和 CategoryID 做為值](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**圖 8**： 已`CategoryName`顯示欄位而`CategoryID`做為值 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image24.png))


進行這些變更的宣告式標記之後`EditItemTemplate`在`CategoryName`TemplateField DropDownList 和 ObjectDataSource，會包含：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> 在 DropDownList`EditItemTemplate`必須啟用其檢視狀態。 我們很快就會將資料繫結語法加入 DropDownList 的宣告式語法和資料繫結命令喜歡`Eval()`和`Bind()`只能出現在已啟用其檢視狀態的控制項。


重複這些步驟來新增名為 DropDownList`Suppliers`要`SupplierName`TemplateField 的`EditItemTemplate`。 這將涉及新增至 DropDownList`EditItemTemplate`並建立另一個的 ObjectDataSource。 `Suppliers` DropDownList 的 ObjectDataSource，不過，應該設定為叫用`SuppliersBLL`類別的`GetSuppliers()`方法。 此外，設定`Suppliers`DropDownList 以顯示`CompanyName`欄位，並使用`SupplierID`欄位的值作為其`ListItem`s。

加入兩個 dropdownlist 進行之後`EditItemTemplate`s，載入瀏覽器頁面，然後按一下 [編輯] 按鈕的 Chef Anton 印地安 Seasoning 產品。 如 [圖 9] 所示，會將產品類別目錄和供應商資料行轉譯為包含可用分類和供應商可從中選擇的下拉式清單中。 但請注意，*第一個*兩份下拉式清單中的項目預設會選取 （飲料類別目錄） 和山為供應商，即使 Chef Anton 印地安 Seasoning 提供紐奧良印地安 Condiment樂趣。


[![預設會選取下拉式清單中第一個項目](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**圖 9**： 預設會選取下拉式清單中的第一個項目 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image27.png))


此外，如果您按一下 [更新] 時，您可找到的產品`CategoryID`並`SupplierID`值都會設為`NULL`。 這兩種意外的行為會導致因為在 dropdownlist 進行`EditItemTemplate`s 未繫結至任何資料欄位從基礎的產品資料。

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>繫結至 dropdownlist 進行`CategoryID`和`SupplierID`資料欄位

若要編輯的產品類別目錄和供應商下拉式清單設定為適當的值，並讓這些值送回到 BLL`UpdateProduct`時按一下 更新方法，我們需要將繫結 dropdownlist 進行`SelectedValue`屬性`CategoryID`和`SupplierID`使用雙向資料繫結的資料欄位。 若要完成這項作業`Categories`下拉式清單中，您可以新增`SelectedValue='<%# Bind("CategoryID") %>'`直接與宣告式語法。

或者，您可以設定 DropDownList databindings 編輯透過設計工具的範本，然後按一下 編輯資料繫結連結，從 DropDownList 的智慧標籤。 接下來，表示`SelectedValue`屬性應該繫結至`CategoryID`欄位使用雙向資料繫結 （請參閱 圖 10）。 重複繫結宣告式或設計工具處理`SupplierID`資料欄位至`Suppliers`DropDownList。


[![將 CategoryID 繫結使用雙向資料繫結 DropDownList SelectedValue 屬性](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**圖 10**： 繫結`CategoryID`至 DropDownList`SelectedValue`屬性使用雙向資料繫結 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image30.png))


若要套用繫結之後`SelectedValue`的兩個 dropdownlist 進行內容中，已編輯的產品類別目錄] 和 [供應商欄將會預設為目前產品的值。 按一下 更新後,`CategoryID`並`SupplierID`下拉式清單中選取的項目的值會傳遞至`UpdateProduct`方法。 [圖 11] 顯示本教學課程之後已加入資料繫結陳述式;請注意如何 Chef Anton 印地安 Seasoning 的下拉式清單中選取項目都正確 Condiment 令紐奧良印地安。


[![編輯產品的目前類別和供應商值預設會選取](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**圖 11**： 根據預設會選取編輯產品的目前類別和供應商值 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>處理`NULL`值

`CategoryID`並`SupplierID`中的資料行`Products`表可以`NULL`，但在 dropdownlist 進行`EditItemTemplate`不包含清單項目來表示`NULL`值。 這有兩種結果：

- 使用者無法使用我們的介面來從非變更產品的分類或供應商`NULL`值`NULL`其中一個
- 如果產品有`NULL``CategoryID`或`SupplierID`，按一下 [編輯] 按鈕時，會導致例外狀況。 這是因為`NULL`所傳回的值`CategoryID`(或`SupplierID`) 中`Bind()`陳述式不會對應至 DropDownList 中的值 (DropDownList 會擲回的例外狀況時其`SelectedValue`屬性設定為值*不*其集合中的清單項目)。

為了支援`NULL``CategoryID`並`SupplierID`值，我們需要加入另一個`ListItem`來代表每個 DropDownList`NULL`值。 在[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程中，我們了解如何新增其他`ListItem`來資料繫結 DropDownList，其中涉及設定 DropDownList`AppendDataBoundItems`屬性設`true`和手動加入 其他`ListItem`。 在先前的教學課程中，不過，我們新增了`ListItem`具有`Value`的`-1`。 資料繫結邏輯在 ASP.NET 中，不過，會自動轉換至空白的字串`NULL`值和相反的。 因此，對於本教學課程中，我們希望`ListItem`的`Value`為空字串。

開始設定這兩個 dropdownlist 進行`AppendDataBoundItems`屬性設`true`。 接下來，新增`NULL``ListItem`加上下列`<asp:ListItem>`DropDownList 每個項目，讓宣告式標記看起來像：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

我選擇使用"(None)"做為文字值這個`ListItem`，但您可以將它也可以是空白字串，如果您想要變更。

> [!NOTE]
> 如我們在中所見*主版/詳細篩選使用 DropDownList*教學課程中， `ListItem` s 可以新增至 DropDownList 透過設計工具，按一下 DropDownList`Items`屬性 視窗中的屬性 （這將會顯示`ListItem`集合編輯器)。 不過，請務必新增`NULL``ListItem`本教學課程中透過宣告式語法。 如果您使用`ListItem`集合編輯器 中，將會省略產生的宣告式語法`Value`一併設定時指派為空白的字串，建立類似的宣告式標記： `<asp:ListItem>(None)</asp:ListItem>`。 雖然這看起來無害，遺漏的值就會使用 DropDownList`Text`在其位置中的屬性值。 這表示，如果這`NULL``ListItem`已選取，"(None)"的值將會嘗試指派給`CategoryID`，從而導致例外狀況。 藉由明確將`Value=""`，則`NULL`值會指派給`CategoryID`時`NULL``ListItem`已選取。


針對供應商 DropDownList 中重複這些步驟。

與這個額外`ListItem`，現在可以將指派的編輯介面`NULL`值的產品`CategoryID`和`SupplierID`欄位，如 圖 12 所示。


[![若要將 NULL 值指派為產品的分類或供應商中選擇 （無）](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**圖 12**: (None) 選擇要指派`NULL`產品的分類或供應商的值 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>步驟 4： 使用選項按鈕已停用狀態

目前產品的`Discontinued`使用 CheckBoxField，呈現的是唯讀的資料列已停用核取方塊和正在編輯之資料列已啟用 核取方塊來表示資料欄位。 通常適合此使用者介面時，我們可以視需要使用 TemplateField 加以自訂。 本教學課程中，讓我們變更 CheckBoxField 」 到 「 作用中 」 和 「 已停止 」 的兩個選項使用 RadioButtonList 控制項，使用者可以從中指定產品的 TemplateField`Discontinued`值。

啟動轉換`Discontinued`CheckBoxField 到 TemplateField，這將會建立使用 TemplateField`ItemTemplate`和`EditItemTemplate`。 這兩個範本包含具有核取方塊及其`Checked`屬性繫結至`Discontinued`資料欄位中，唯一的差別，在於兩者之間`ItemTemplate`的核取方塊之`Enabled`屬性設定為`false`。

取代的核取方塊，在這兩`ItemTemplate`並`EditItemTemplate`RadioButtonList 控制項中，設定這兩個 RadioButtonLists'`ID`屬性，以`DiscontinuedChoice`。 接下來，表示 RadioButtonLists 應該每個包含兩個選項按鈕，一個標示為 「 作用中 」 值為"False"並標示為 「 已停止 」 值為"True"。 若要完成此您可以輸入`<asp:ListItem>`中的項目，直接透過宣告式語法或使用`ListItem`從設計工具的集合編輯器。 [圖 13] 顯示`ListItem`已指定集合編輯器後兩個選項按鈕的選項。


[![新增](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**圖 13**： 將 「 作用中 」 和 「 Discontinued 」 選項新增至 RadioButtonList ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image39.png))


因為在 RadioButtonList`ItemTemplate`不應該是可編輯，設定其`Enabled`屬性設`false`、 離開`Enabled`屬性設`true`（預設值） 中 RadioButtonList 的`EditItemTemplate`。 這會讓非編輯資料列，以唯讀模式，選項按鈕，但可讓使用者變更編輯資料列的 RadioButton 值。

我們仍需要指派 RadioButtonList 控制項的`SelectedValue`屬性選取適當的選項按鈕，以便根據產品的`Discontinued`資料欄位。 如同稍早在本教學課程中檢查 dropdownlist 進行，此資料繫結語法會加入到宣告式標記的直接或透過 RadioButtonLists 的智慧標籤中的 [編輯資料繫結] 連結。

在新增兩個 RadioButtonLists 和設定它們之後, `Discontinued` TemplateField 的宣告式標記應該看起來像：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

經過這些變更，`Discontinued`清單 （請參閱 圖 14） 的選項按鈕配對的資料行轉換從清單中的核取方塊。 當您編輯產品，選取適當的選項按鈕，可以更新的產品已停止的狀態，選取 其他 選項按鈕，然後按一下 更新。


[![選項按鈕組已取代的已停用核取方塊](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**圖 14**: 停用核取方塊已被取代選項按鈕組 ([按一下以檢視完整大小的影像](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> 由於`Discontinued`中的資料行`Products`資料庫不能有`NULL`的值，我們不需要擔心如何擷取`NULL`介面中的資訊。 如果，不過，`Discontinued`資料行可能包含`NULL`值，我們會想要新增第三個選項按鈕，以填入清單及其`Value`設為空字串 (`Value=""`)，就像使用類別和供應商 dropdownlist 進行。


## <a name="summary"></a>總結

雖然 BoundField 及其自動呈現唯讀、 編輯和插入介面，缺乏自訂能力。 通常，不過，我們需要自訂的編輯，或插入介面，或許將驗證控制項新增 （如我們在先前的教學課程中所見的） 或自訂資料集合的使用者介面 （如我們在本教學課程中所見的）。 自訂使用 TemplateField 介面可以被加總在下列步驟：

1. 新增為 TemplateField 或現有 BoundField 或 CheckBoxField 轉換為 TemplateField
2. 視需要擴充介面
3. 將適當的資料欄位繫結至新加入的 Web 控制項使用雙向資料繫結

除了使用內建的 ASP.NET Web 控制項，您也可以自訂為 TemplateField 使用自訂的已編譯的伺服器控制項和使用者控制項的範本。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [下一頁](implementing-optimistic-concurrency-cs.md)
