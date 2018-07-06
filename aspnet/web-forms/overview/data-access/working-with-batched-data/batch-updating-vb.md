---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: 批次更新 (VB) |Microsoft Docs
author: rick-anderson
description: 了解如何更新單一作業中的多個資料庫記錄。 在使用者介面層，我們會建置的 GridView，其中每個資料列都可以讓您編輯。 在資料...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e5062898ca683571df2929eba5d824f9d77accd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833397"
---
<a name="batch-updating-vb"></a>批次更新 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip)或[下載 PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> 了解如何更新單一作業中的多個資料庫記錄。 在使用者介面層，我們會建置的 GridView，其中每個資料列都可以讓您編輯。 資料存取層中，我們換行以確保所有的更新成功，或所有更新會都回復在交易內的多個更新作業的內容。


## <a name="introduction"></a>簡介

在 [前述教學課程](wrapping-database-modifications-within-a-transaction-vb.md)我們了解如何擴充資料的存取層，來新增的資料庫交易支援。 資料庫交易保證一系列的資料修改陳述式會被視為一個不可部分完成的作業，可確保所有修改將會都失敗，或所有將會成功。 有了這個低階 DAL 功能的方式，我們重新已準備好把焦點轉到建立批次資料修改介面。

在本教學課程中，我們將建置的 GridView，其中每個資料列是可編輯 （請參閱 圖 1）。 由於不需要編輯的資料行時，會在其編輯介面上，該處 s 呈現每個資料列，更新和 [取消] 按鈕。 相反地，有兩個更新的產品按鈕在頁面上，按一下時，列舉的 GridView 資料列，並更新資料庫。


[![在 gridview 裡的每個資料列是可編輯](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**圖 1**： 在 gridview 裡的每個資料列是可編輯 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image2.png))


讓 s 開始 ！

> [!NOTE]
> 在 [執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)教學課程中我們建立批次的編輯介面使用 DataList 控制項。 不同於先前的其中一個，會使用 GridView 的本教學課程中，批次執行更新時的交易範圍內。 完成本教學課程之後我鼓勵您返回先前的教學課程，並加以更新才能使用資料庫交易相關功能加入在先前的教學課程。


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>檢查進行所有的 GridView 資料列的可編輯的步驟

中所述[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，GridView 提供編輯每個資料列依其基礎資料的內建支援。 就內部而言，GridView 資訊是透過可編輯的哪些資料列及其[`EditIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)。 GridView 繫結至其資料來源，因為它會檢查每個資料列，以查看資料列的索引若等於值`EditIndex`。 如果是的話，該資料列的欄位經轉譯之後使用其編輯介面。 編輯介面是文字方塊 BoundFields，其`Text`BoundField s 所指定的資料欄位的值指派給屬性`DataField`屬性。 TemplateFields，如`EditItemTemplate`用來取代`ItemTemplate`。

您應該記得編輯工作流程會啟動，當使用者按一下資料列 s [編輯] 按鈕。 這會導致回傳，設定 GridView 的`EditIndex`屬性設為 s 的已按下 資料列索引，並重新繫結至方格的資料。 按一下資料列 s [取消] 按鈕的時，可在回傳`EditIndex`設定的值為`-1`重新繫結至方格的資料。 GridView s 資料列開始編製索引為零，因為設定`EditIndex`至`-1`以唯讀模式顯示 GridView 的效果。

`EditIndex`屬性適用於每個資料列編輯，但不是針對批次的編輯。 若要讓整個 GridView 可進行編輯，我們需要能夠使用其編輯介面呈現每個資料列。 若要這麼做最簡單的方式是建立其中每個可編輯的欄位會實作為具有其編輯介面為 TemplateField 中定義`ItemTemplate`。

接下來幾個步驟我們將建立完全可編輯的 GridView。 在步驟 1 中我們將開始建立 GridView 和其 ObjectDataSource，並將其 BoundFields 及其轉換成 TemplateFields。 步驟 2 和 3 中，我們會說明的編輯介面從 TemplateFields`EditItemTemplate`至其`ItemTemplate`s。

## <a name="step-1-displaying-product-information"></a>步驟 1︰ 顯示產品資訊

我們會擔心建立 GridView 之前所在的資料列都可以編輯，讓 s 首先會只顯示產品資訊。 開啟`BatchUpdate.aspx`頁面中`BatchData`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳的 GridView。 設定 GridView s`ID`要`ProductsGrid`，並從它的智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定要擷取其資料從 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。


[![設定使用 ProductsBLL 類別 ObjectDataSource](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**圖 2**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image4.png))


[![擷取產品的資料使用 GetProducts 方法](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**圖 3**： 擷取產品資料使用`GetProducts`方法 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image6.png))


GridView，之類的 ObjectDataSource 的修改功能專為是每個資料列的基礎。 若要更新的一組記錄，我們必須撰寫的程式碼的批次資料，並將它傳遞給 BLL，ASP.NET 頁面 s 程式碼後置類別中。 因此，設定下拉式清單中之 ObjectDataSource 更新、 插入和刪除的索引標籤，為 （無）。 按一下 完成 以完成精靈。


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**圖 4**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image8.png))


完成設定資料來源精靈之後，ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

完成設定資料來源精靈 時，也會讓 Visual Studio 在 gridview 裡建立 BoundFields 及其產品資料欄位。 本教學課程中，可讓 s 只允許使用者檢視及編輯產品的名稱、 類別、 價格和已停止的狀態。 移除以外的所有`ProductName`， `CategoryName`， `UnitPrice`，以及`Discontinued`欄位和重新命名`HeaderText`的前三個屬性分別欄位加入產品 」、 「 類別目錄和 「 價格 」。 最後，核取方塊啟用分頁，並啟用排序 GridView s 智慧標籤中。

此時 GridView 有三個 BoundFields (`ProductName`， `CategoryName`，並`UnitPrice`) 及其 (`Discontinued`)。 我們要將這四個欄位轉換成 TemplateFields，然後再將編輯介面移 s 中的 TemplateField`EditItemTemplate`至其`ItemTemplate`。

> [!NOTE]
> 我們已探索的建立和自訂中的 TemplateFields[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程。 我們將逐步解說的步驟，將 BoundFields 及其轉換成 TemplateFields 和定義其編輯介面中其`ItemTemplate`s，但如果卡住或需要重新整理程式，不要遲疑，立即以回頭參考此先前的教學課程。


從 GridView s 智慧標籤，按一下 編輯資料行連結以開啟 欄位 對話方塊。 接下來，選取每個欄位，並按一下 TemplateField 連結轉換此欄位。


![將現有的 BoundFields 及其轉換成 TemplateFields](batch-updating-vb/_static/image5.gif)

**圖 5**： 將現有的 BoundFields 及其轉換成 TemplateFields


現在，每個欄位都為 TemplateField，我們準備好要移動的編輯介面從`EditItemTemplate`s `ItemTemplate` s。

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>步驟 2： 建立`ProductName`，`UnitPrice`，和`Discontinued`編輯介面

建立`ProductName`， `UnitPrice`，並`Discontinued`編輯介面是此步驟的主題和 TemplateField s 中已定義的每個介面都非常簡單， `EditItemTemplate`。 建立`CategoryName`編輯介面是稍微複雜一點，因為我們需要建立一個適用的類別目錄的 DropDownList。 這`CategoryName`編輯介面在步驟 3 中的方式來處理。

將開頭的 s `ProductName` TemplateField。 按一下 [GridView] s 智慧標籤中的 [編輯範本] 連結，然後向下切入至`ProductName`TemplateField 的`EditItemTemplate`。 選取文字方塊中，將它複製到剪貼簿，然後貼到`ProductName`TemplateField 的`ItemTemplate`。 變更文字方塊 s`ID`屬性設`ProductName`。

接下來，新增至 RequiredFieldValidator`ItemTemplate`以確保使用者提供的值，針對每個產品 s 的名稱。 設定`ControlToValidate`ProductName，屬性`ErrorMessage`屬性，您必須提供產品的名稱。 而`Text`屬性設\*。 進行這些新增項目後`ItemTemplate`，您的畫面應該看起來會類似 圖 6。


[![ProductName TemplateField 現在包含文字方塊和 RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**圖 6**: `ProductName` TemplateField 現在包含一個文字方塊和 RequiredFieldValidator ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image10.png))


針對`UnitPrice`編輯介面，開始藉由複製文字方塊中的，從`EditItemTemplate`至`ItemTemplate`。 接下來，將 $ 前面的文字方塊和設定其`ID`UnitPrice 屬性並將其`Columns`8 的屬性。

也將新增至 CompareValidator `UnitPrice` s`ItemTemplate`以確保使用者輸入的值是有效的貨幣值大於或等於到美金 $0.00 元。 設定驗證程式 s `ControlToValidate` UnitPrice 屬性其`ErrorMessage`屬性，您必須輸入有效的貨幣值。 請省略任何貨幣符號。，其`Text`屬性，以\*、 其`Type`屬性設`Currency`、 其`Operator`屬性設`GreaterThanEqual`，並將其`ValueToCompare`屬性設為 0。


[![新增 CompareValidator，以確保價格輸入為非負數貨幣值](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**圖 7**： 新增 CompareValidator，以確保價格輸入為非負數貨幣值 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image12.png))


針對`Discontinued`TemplateField，您可以使用已經定義中的核取方塊`ItemTemplate`。 只要將設定其`ID`Discontinued 至及其`Enabled`屬性設`True`。

## <a name="step-3-creating-thecategorynameediting-interface"></a>步驟 3： 建立`CategoryName`編輯介面

中的編輯介面`CategoryName`TemplateField s`EditItemTemplate`包含文字方塊中顯示的值`CategoryName`資料欄位。 我們要取代此項使用 dropdownlist 進行列出可能的類別。

> [!NOTE]
> [自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程包含自訂範本以包含相對於文字方塊 dropdownlist 進行更徹底且更完整討論。 這裡的步驟完成時，他們會看到 tersely。 如建立和設定的分類 DropDownList 需深入了解，請參閱上一步[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程。


從工具箱拖曳 DropDownList `CategoryName` TemplateField s `ItemTemplate`，將其`ID`至`Categories`。 此時我們會通常定義 dropdownlist 進行的資料來源，透過它的智慧標籤，建立新的 ObjectDataSource。 不過，這會將新增內的 ObjectDataSource `ItemTemplate`，從而導致每個 GridView 資料列所建立的 ObjectDataSource 執行個體。 相反地，讓建立 ObjectDataSource GridView 的 TemplateFields 之外。 結束範本編輯，並從 [工具箱] 拖曳至設計工具下方拖曳 ObjectDataSource `ProductsDataSource` ObjectDataSource。 名稱的新 ObjectDataSource`CategoriesDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategories`方法。


[![設定使用 CategoriesBLL 類別 ObjectDataSource](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**圖 8**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image14.png))


[![擷取使用 GetCategories 方法的類別目錄資料](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**圖 9**： 擷取類別目錄資料使用`GetCategories`方法 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image16.png))


由於這個 ObjectDataSource 只是用以擷取資料，設定下拉式清單中的 UPDATE 和 DELETE 的索引標籤為 （無）。 按一下 完成 以完成精靈。


[![設定下拉式清單中的更新和刪除索引標籤，為 （無）](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**圖 10**： 設定下拉式清單中的更新和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image18.png))


在精靈中，完成後`CategoriesDataSource`s 宣告式標記應該如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

具有`CategoriesDataSource`建立並設定，回到`CategoryName`TemplateField 的`ItemTemplate`並從 DropDownList s 智慧標籤，按一下 選擇資料來源連結。 在 [資料來源組態精靈] 中，選取`CategoriesDataSource`從第一個下拉式清單選項，然後選擇`CategoryName`用於顯示和`CategoryID`做為值。


[![繫結至 CategoriesDataSource 的 DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**圖 11**： 繫結至 DropDownList `CategoriesDataSource` ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image20.png))


此時`Categories`DropDownList 列出所有類別，但它不會還會自動選取適當的類別，繫結至 GridView 資料列的產品。 若要這麼做我們需要設定`Categories`DropDownList s`SelectedValue`產品 s`CategoryID`值。 按一下 從 DropDownList s 智慧標籤的 編輯資料繫結 連結，並將產生關聯`SelectedValue`屬性與`CategoryID`資料欄位，如 圖 12 所示。


![將產品的 CategoryID 值繫結 DropDownList 的 SelectedValue 屬性](batch-updating-vb/_static/image12.gif)

**圖 12**： 繫結產品 s `CategoryID` DropDownList s 值`SelectedValue`屬性


一個過去的問題會維持： 有的產品不 t`CategoryID`值，然後在資料繫結陳述式上指定`SelectedValue`會導致例外狀況。 這是因為 DropDownList 包含類別的項目，而且不提供這些產品的選項`NULL`資料庫值`CategoryID`。 若要解決此問題，將設定 DropDownList s`AppendDataBoundItems`屬性，以`True`並將新的項目新增至下拉式清單中，省略`Value`從宣告式語法的屬性。 也就是確定`Categories`DropDownList s 宣告式語法看起來如下：


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

請注意如何`<asp:ListItem Value="">`-選取一個-具有其`Value`屬性明確設為空字串。 回頭[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)如需為何需要這個額外的 DropDownList 項目用來處理的更完整討論教學課程`NULL`案例和原因指派`Value`屬性設為空字串是不可或缺的。

> [!NOTE]
> 還有潛在的效能和延展性問題這裡值得一提的。 因為每個資料列有使用 DropDownList`CategoriesDataSource`當做其資料來源，`CategoriesBLL`類別 s`GetCategories`方法將會呼叫*n*瀏覽每個頁面的時間，其中*n*是數目在 gridview 裡的資料列。 這些*n*呼叫`GetCategories`導致*n*向資料庫查詢。 無法減少這種影響的資料庫，藉由快取傳回的類別目錄的每個要求快取中或透過使用 SQL 快取相依性或極短時間為基礎的過期快取層級。 如需有關每個要求快取選項，請參閱[`HttpContext.Items`每個要求的快取存放區](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx)。


## <a name="step-4-completing-the-editing-interface"></a>步驟 4： 完成編輯介面

我們 ve 範本所做的變更數目 GridView s 停頓檢視我們的進度。 請花一點時間檢閱我們透過瀏覽器的進度。 如 [圖 13] 所示，每個資料列使用呈現其`ItemTemplate`，其中包含儲存格 s 中的編輯介面。


[![每個 GridView 資料列是可編輯](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**圖 13**： 每個 GridView 資料列是可編輯 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image22.png))


有一些次要的格式設定問題，我們會負責在此時。 首先，請注意，`UnitPrice`值包含四個小數位數。 若要修正此問題，傳回到`UnitPrice`TemplateField 的`ItemTemplate`和從文字方塊 s 智慧標籤，按一下 [編輯資料繫結] 連結。 接下來，指定`Text`屬性的格式應該是數字。


![表示為數字格式的文字屬性](batch-updating-vb/_static/image14.gif)

**圖 14**： 格式`Text`屬性，表示為數字


第二，可讓 s center 中的核取`Discontinued`資料行 （而非它靠左對齊）。 從 GridView s 智慧標籤中按一下 編輯資料行，然後選取`Discontinued`TemplateField 從左下角中的欄位清單。 向下鑽研`ItemStyle`並設定`HorizontalAlign`Center，如 圖 15 所示的屬性。


![置中已停止的核取方塊](batch-updating-vb/_static/image15.gif)

**圖 15**: Center`Discontinued`核取方塊


接下來，ValidationSummary 控制項加入頁面，並設定其`ShowMessageBox`屬性，以`True`及其`ShowSummary`屬性設`False`。 也將新增按鈕 Web 控制項，按下時，將會更新使用者的變更。 具體來說，新增兩個按鈕 Web 控制項，一個以上的 GridView，另一個下方，設定兩個控制項`Text`更新產品的屬性。

GridView s 自編輯介面定義於其 TemplateFields `ItemTemplate` s， `EditItemTemplate` s 是多餘的而且會遭到刪除。

進行上述提到的格式設定變更之後，新增按鈕控制項，並移除不必要`EditItemTemplate`s，您的頁面 s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

圖 16 顯示此頁面已加入 Button Web 控制項之後，透過瀏覽器檢視時，並格式化所做的變更。


[![頁面現在包含兩個更新的產品按鈕](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**圖 16**: 頁面現在包含兩個更新的產品按鈕 ([按一下以檢視完整大小的影像](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>步驟 5： 更新產品

當使用者造訪此頁面他們進行修改，然後按一下其中一個的兩個更新產品按鈕。 此時，我們需要以某種方式儲存至每個資料列的使用者輸入值`ProductsDataTable`執行個體，並再將它傳遞至再經歷，BLL 方法`ProductsDataTable`DAL s 執行個體`UpdateWithTransaction`方法。 `UpdateWithTransaction`方法，我們在建立[前述教學課程](wrapping-database-modifications-within-a-transaction-vb.md)，可確保批次的變更將會更新以不可部分完成的作業。

建立名為的方法`BatchUpdate`在`BatchUpdate.aspx.vb`並加入下列程式碼：


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

這個方法一開始所取得的所有產品年代`ProductsDataTable`BLL 的呼叫透過`GetProducts`方法。 接著它會列舉`ProductGrid`GridView s [ `Rows`集合](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)。 `Rows`集合包含[`GridViewRow`執行個體](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx)GridView 中顯示每個資料列。 因為我們會顯示最多 10 個資料列每一頁，GridView 的`Rows`集合中會有不超過十個項目。

每個資料列`ProductID`捕捉來自`DataKeys`收集和適當`ProductsRow`從選取`ProductsDataTable`。 以程式設計方式所參考的四個的 TemplateField 輸入的控制項和其值指派給`ProductsRow`執行個體的內容。 之後每個 GridView 資料列的值已用來更新`ProductsDataTable`，它 s 傳遞到 BLL`UpdateWithTransaction`方法，如我們所見在先前的教學課程中，只要呼叫向下到 DAL 的`UpdateWithTransaction`方法。

本教學課程使用的批次更新演算法更新中的每個資料列`ProductsDataTable`對應至 GridView，不論是否已變更的產品的資訊中的資料列。 而這類 blind 更新通常並不是效能問題，它們可能會導致過多記錄您稽核變更為資料庫資料表。 回到[執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)教學課程中我們探討更新介面與批次，並新增程式碼，只會更新使用者實際修改這些記錄。 歡迎使用從技術[執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)如有需要，在本教學課程中，更新程式碼。

> [!NOTE]
> Visual Studio 時繫結到它的智慧標籤 GridView 資料來源，會自動指派到 GridView 資料來源 s 主要值`DataKeyNames`屬性。 如果您未繫結 ObjectDataSource GridView 透過 GridView s 智慧標籤在步驟 1 中所述，則您必須手動設定 GridView s`DataKeyNames`若要存取的 ProductID 屬性`ProductID`透過每個資料列的值`DataKeys`集合。


中所使用的程式碼`BatchUpdate`類似於用於 BLL s`UpdateProduct`方法，最主要的差異，在於中`UpdateProduct`方法只會有一個`ProductRow`擷取執行個體的架構。 指派的屬性的程式碼`ProductRow`之間相同`UpdateProducts`方法和中的程式碼`For Each`迴圈`BatchUpdate`，因為是整體模式。

若要完成本教學課程中，我們必須能夠`BatchUpdate`時叫用方法的更新產品按鈕按下。 建立事件處理常式`Click`事件，這兩個按鈕控制項，然後在 事件處理常式中新增下列程式碼：


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

第一次進行呼叫，以`BatchUpdate`。 下一步 [ `ClientScript`屬性](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx)可用來插入 JavaScript，其中會顯示 messagebox，以讀取已更新的產品。

花點時間試用此程式碼。 請瀏覽`BatchUpdate.aspx`透過瀏覽器中，編輯資料列數，再按一下其中一個更新產品按鈕。 假設沒有任何輸入的驗證錯誤，您應該會看到 messagebox 讀取已更新的產品。 若要確認更新的不可部分完成性，請考慮新增的隨機`CHECK`條件約束，例如不允許`UnitPrice`1234.56 的值。 接著從`BatchUpdate.aspx`，編輯的記錄數，並確認其設定其中一個產品的`UnitPrice`禁止的值 (1234.56) 的值。 這應該導致發生錯誤時，批次作業時，按一下與其他變更的更新產品回復到其原始值。

## <a name="an-alternativebatchupdatemethod"></a>替代`BatchUpdate`方法

`BatchUpdate`方法，我們只是檢查擷取*所有*的產品，從 BLL 的`GetProducts`方法，然後更新只會出現在 GridView 的記錄。 如果 GridView 不會使用分頁，但如果是這樣，可能有數百個、 幾千個或數以萬計的產品，但是在 gridview 裡的只有 10 個資料列，則這個方法會很理想。 在此情況下，從只是要修改 10 個的資料庫取得所有產品都是不盡理想。

對於這些類型的情況中，請考慮使用下列`BatchUpdateAlternate`方法改為：


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` 開始建立新的空`ProductsDataTable`名為`products`。 然後 GridView 的逐步`Rows`集合，在每個資料列取得特定產品資訊使用 BLL 的`GetProductByProductID(productID)`方法。 擷取`ProductsRow`執行個體有相同的方式，做為更新的屬性`BatchUpdate`，但在更新匯入的資料列之後`products``ProductsDataTable`經由 DataTable s [ `ImportRow(DataRow)`方法](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

在後`For Each`迴圈完成時，`products`包含一個`ProductsRow`GridView 裡的每個資料列的執行個體。 因為每個`ProductsRow`執行個體已新增至`products`（而不是更新），如果我們盲目地將它傳遞給`UpdateWithTransaction`方法`ProductsTableAdatper`會嘗試將每筆記錄插入資料庫。 相反地，我們需要指定，這些資料列的每個已修改 （未加入）。

這可藉由將新方法新增至名為 BLL `UpdateProductsWithTransaction`。 `UpdateProductsWithTransaction`如下所示，設定`RowState`的每個`ProductsRow`中的執行個體`ProductsDataTable`來`Modified`，並接著傳遞`ProductsDataTable`DAL s`UpdateWithTransaction`方法。


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>總結

GridView 會提供內建的每列的編輯功能，但不支援建立完整的可編輯的介面。 如我們所見本教學課程中，這類介面是可行的但需要一些工作。 若要建立其中每個資料列是可編輯的 GridView，我們需要將 GridView 的欄位轉換成 TemplateFields 並定義內的編輯介面`ItemTemplate`s。 此外，更新所有按鈕 Web 控制項的型別必須新增至頁面上，個別從 GridView。 這些按鈕`Click`事件處理常式需要列舉的 GridView s`Rows`集合中，存放區中的變更`ProductsDataTable`，並將更新的資訊傳遞至適當的 BLL 方法。

在下一個教學課程中，我們將了解如何建立批次刪除介面。 特別的是，每個 GridView 資料列會包含一個核取方塊並而不是全部更新-輸入按鈕，我們必須刪除選取的資料列按鈕。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 David Suru。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](wrapping-database-modifications-within-a-transaction-vb.md)
> [下一頁](batch-deleting-vb.md)
