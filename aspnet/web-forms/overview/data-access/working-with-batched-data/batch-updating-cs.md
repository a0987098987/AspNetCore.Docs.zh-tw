---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: "批次更新 (C#) |Microsoft 文件"
author: rick-anderson
description: "了解如何更新在單一作業中的多個資料庫記錄。 在使用者介面層級中，我們會建置其中每個資料列都可編輯的 GridView。 在資料..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 506ecc9fad47cc39a0323e9ed18814c26e28ee47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="batch-updating-c"></a>批次更新 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip)或[下載 PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> 了解如何更新在單一作業中的多個資料庫記錄。 在使用者介面層級中，我們會建置其中每個資料列都可編輯的 GridView。 資料存取層中我們會換行以確保所有更新會都成功，或所有更新會都回復在交易內的多個更新作業。


## <a name="introduction"></a>簡介

在[前述教學課程](wrapping-database-modifications-within-a-transaction-cs.md)我們可了解如何將延伸資料的存取層，若要加入的資料庫交易支援。 資料庫交易保證一系列的資料修改陳述式將會被視為一個不可部分完成的作業，可確保所有的修改將會失敗，或所有將會成功。 利用此低階 DAL 功能的方式，我們重新已準備好開啟我們注意到建立批次資料修改的介面。

在本教學課程中，我們會建置的 GridView，其中每個資料列都可編輯 （請參閱圖 1）。 因為編輯的資料行不需要時，會在其編輯介面上，該處 s 轉譯每個資料列，更新和取消按鈕。 相反地，有兩個更新的產品按鈕頁面上，按一下時，列舉的 GridView 資料列，並更新資料庫。


[![在 GridView 中的每個資料列是編輯](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**圖 1**: GridView 中的每個資料列是編輯 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image2.png))


Let s 開始 ！

> [!NOTE]
> 在[執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)教學課程中我們建立批次編輯介面使用 DataList 控制項。 本教學課程會不同於先前的其中一個，是使用 GridView 且在交易範圍內執行批次更新。 完成本教學課程之後建議您將返回先前的教學課程，並更新它使用的資料庫與交易相關功能加入在先前的教學課程。


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>檢查進行所有的 GridView 資料列可編輯的步驟

中所述[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教學課程中，GridView 提供編輯每個資料列依其基礎資料的內建支援。 就內部而言，GridView 記哪些資料列是透過可編輯其[`EditIndex`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)。 GridView 繫結至其資料來源，因為它會檢查以查看的資料列索引是否等於值的每個資料列`EditIndex`。 如果是的話，該資料列的欄位就會使用呈現其編輯介面。 編輯介面是文字方塊 BoundFields，其`Text`BoundField s 所指定的資料欄位的值指派給屬性`DataField`屬性。 TemplateFields，如`EditItemTemplate`用來取代`ItemTemplate`。

前文提過編輯工作流程會啟動，當使用者按一下資料列 s [編輯] 按鈕。 這會導致回傳，請設定 GridView 的`EditIndex`屬性設為按下後的資料列的索引並重新繫結至資料格的資料。 資料列 s [取消] 按鈕按一下的時間，在回傳`EditIndex`設定的值為`-1`重新繫結至資料格的資料。 GridView s 資料列開始索引零，因為設定`EditIndex`至`-1`在唯讀模式中顯示的 GridView 的效果。

`EditIndex`屬性適用於每個資料列編輯，但不是編輯批次。 若要讓整個 GridView 可編輯，我們必須將每個資料列呈現使用其編輯介面。 完成這項作業的最簡單方式是建立每個可編輯欄位，實作中定義其編輯介面具有為 TemplateField `ItemTemplate`。

下一步 透過幾個步驟我們將建立完全可編輯的 GridView。 在步驟 1 中我們將開始建立 GridView 以及其 ObjectDataSource，並將其 BoundFields 和 CheckBoxField 轉換成 TemplateFields。 步驟 2 和 3 中，我們會說明編輯介面從 TemplateFields`EditItemTemplate`以其`ItemTemplate`s。

## <a name="step-1-displaying-product-information"></a>步驟 1： 顯示產品資訊

我們會擔心建立 GridView 之前的資料列都可以編輯，讓 s 首先會只顯示產品資訊。 開啟`BatchUpdate.aspx`頁面`BatchData`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 GridView。 設定 GridView s`ID`至`ProductsGrid`和從其智慧標籤上，選擇 繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定要擷取其資料從 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。


[![設定使用 ProductsBLL 類別 ObjectDataSource](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**圖 2**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image4.png))


[![擷取產品資料使用 GetProducts 方法](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**圖 3**： 擷取產品資料使用`GetProducts`方法 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image6.png))


GridView，像是 ObjectDataSource 的修改功能專為每個資料列為基礎。 若要更新的一組記錄，我們需要在批次的資料，並將其傳遞給 BLL ASP.NET 頁面 s 程式碼後置類別中撰寫許多程式碼。 因此，下拉式清單中設定 ObjectDataSource s 為 (None) UPDATE、 INSERT 和 DELETE 的索引標籤。 按一下 [完成] 5d; 以完成精靈。


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**圖 4**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image8.png))


完成設定資料來源精靈之後，ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

完成設定資料來源精靈也會造成 Visual Studio 建立 BoundFields 及其產品資料欄位在 GridView。 本教學課程，可讓 s 只允許使用者檢視和編輯產品的名稱、 類別、 價格和已停止的狀態。 移除以外的所有`ProductName`， `CategoryName`， `UnitPrice`，和`Discontinued`欄位和重新命名`HeaderText`的前三個屬性欄位產品、 類別和價格，分別。 最後，請檢查 GridView s 智慧標籤中的啟用分頁，並啟用排序核取方塊。

在 GridView 此時有三個 BoundFields (`ProductName`， `CategoryName`，和`UnitPrice`) 及其 (`Discontinued`)。 我們需要將這四個欄位轉換成 TemplateFields，然後將編輯介面移從 TemplateField s`EditItemTemplate`至其`ItemTemplate`。

> [!NOTE]
> 我們已探索的建立和自訂 TemplateFields 中的[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程。 我們將逐步 BoundFields 及其轉換 TemplateFields 步驟，並定義其編輯介面在其`ItemTemplate`s，但如果您碰到或需要重新整理程式，不要直接以回頭參考此較早的教學課程。


從 GridView s 智慧標籤，按一下 編輯資料行連結以開啟 欄位 對話方塊。 接下來，選取每個欄位，並按一下轉換此欄位為 TemplateField 連結。


![現有 BoundFields 及其轉換為 TemplateField](batch-updating-cs/_static/image5.gif)

**圖 5**： 現有 BoundFields 及其轉換為 TemplateField


現在，每個欄位都為 TemplateField，我們準備好要移動的編輯介面從`EditItemTemplate`s `ItemTemplate` s。

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>步驟 2： 建立`ProductName`，`UnitPrice`，和`Discontinued`編輯介面

建立`ProductName`， `UnitPrice`，和`Discontinued`編輯介面此步驟中的主題，而很簡單，因為每個介面已經定義於 TemplateField 的`EditItemTemplate`。 建立`CategoryName`編輯介面是稍微複雜，因為我們要建立 DropDownList 一個適用的類別目錄。 這`CategoryName`編輯介面在步驟 3 中處理。

將開頭的 s `ProductName` TemplateField。 按一下 [編輯樣板] 連結，從 GridView s 智慧標籤，然後向下鑽研至`ProductName`TemplateField 的`EditItemTemplate`。 選取文字方塊中，將它複製到剪貼簿，然後將它貼到`ProductName`TemplateField 的`ItemTemplate`。 變更文字方塊 s`ID`屬性`ProductName`。

接下來，加入至 RequiredFieldValidator`ItemTemplate`以確保使用者提供值，為每個產品 s 的名稱。 設定`ControlToValidate`屬性 ProductName，`ErrorMessage`屬性，您必須提供產品的名稱。 和`Text`屬性\*。 進行這些新增項目後`ItemTemplate`，畫面看起來應該類似於圖 6。


[![ProductName TemplateField 現在包含文字方塊和 RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**圖 6**: `ProductName` TemplateField 現在包含文字方塊和 RequiredFieldValidator ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image10.png))


如`UnitPrice`藉由複製從文字方塊中編輯介面，開始`EditItemTemplate`至`ItemTemplate`。 接下來，將 $ 前面的文字方塊和設定其`ID`UnitPrice 的屬性和其`Columns`8 的屬性。

也新增至 CompareValidator `UnitPrice` s`ItemTemplate`以確保使用者所輸入的值大於或等於 $0.00 有效貨幣值。 設定驗證程式 s`ControlToValidate`屬性 UnitPrice，其`ErrorMessage`屬性，您必須輸入有效的貨幣值。 請省略任何貨幣符號。，其`Text`屬性\*、 其`Type`屬性`Currency`、 其`Operator`屬性`GreaterThanEqual`，且其`ValueToCompare`屬性設為 0。


[![加入 CompareValidator 確保價格輸入非負值的貨幣值](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**圖 7**： 加入 CompareValidator 確保價格輸入非負值的貨幣值 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image12.png))


如`Discontinued`TemplateField，您可以使用已經定義中的核取方塊`ItemTemplate`。 直接將其`ID`至 Discontinued 及其`Enabled`屬性`true`。

## <a name="step-3-creating-thecategorynameediting-interface"></a>步驟 3： 建立`CategoryName`編輯介面

中的編輯介面`CategoryName`TemplateField s`EditItemTemplate`包含顯示之值的文字方塊`CategoryName`資料欄位。 我們需要列出可能的類別目錄的 DropDownList 以取代此項。

> [!NOTE]
> [自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程包含自訂範本以包含而不是 TextBox 的 DropDownList 更徹底且更完整討論。 這裡的步驟完成時，他們會看到 tersely。 如建立並設定分類 DropDownList 更深入探討，請參閱上一步[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程。


從工具箱拖曳至拖曳 DropDownList `CategoryName` TemplateField s `ItemTemplate`，設定其`ID`至`Categories`。 此時我們會定義通常 DropDownLists 的資料來源透過其智慧標籤上，建立新的 ObjectDataSource。 不過，這會增加內 ObjectDataSource `ItemTemplate`，以產生每個 GridView 資料列建立 ObjectDataSource 執行個體。 相反地，可讓建立 GridView 的 TemplateFields 之外 ObjectDataSource s。 結束樣板編輯，然後從 [工具箱] 拖曳至設計工具下方拖曳 ObjectDataSource `ProductsDataSource` ObjectDataSource。 命名新 ObjectDataSource`CategoriesDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategories`方法。


[![設定要使用 CategoriesBLL Clas ObjectDataSource](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**圖 8**： 設定要使用 ObjectDataSource `CategoriesBLL` Clas ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image14.png))


[![擷取使用 GetCategories 方法的類別目錄資料](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**圖 9**： 擷取類別目錄資料使用`GetCategories`方法 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image16.png))


因為此 ObjectDataSource 僅用來擷取資料，設定下拉式清單中更新和刪除索引標籤，為 （無）。 按一下 [完成] 5d; 以完成精靈。


[![設定下拉式清單，更新和刪除索引標籤，為 （無）](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**圖 10**： 下拉式清單中設定的更新和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image18.png))


完成精靈之後`CategoriesDataSource`s 宣告式標記應該看起來如下：


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

與`CategoriesDataSource`建立及設定，回到`CategoryName`TemplateField 的`ItemTemplate`和 DropDownList s 智慧標籤，按一下 選擇資料來源連結。 在資料來源組態精靈中，選取`CategoriesDataSource`從第一個下拉式清單選項，然後選擇有`CategoryName`用來顯示和`CategoryID`做為值。


[![繫結至 CategoriesDataSource 的 DropDownList](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**圖 11**： 繫結至 DropDownList `CategoriesDataSource` ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image20.png))


此時`Categories`DropDownList 列出所有類別，但是它不會還會自動選取適當的類別繫結至 GridView 資料列的產品。 若要完成此我們需要設定`Categories`DropDownList s`SelectedValue`產品 s`CategoryID`值。 按一下 編輯資料繫結連結從 DropDownList s 智慧標籤，並建立關聯`SelectedValue`屬性`CategoryID`資料欄位，圖 12 中所示。


![將產品的 CategoryID 值繫結至 DropDownList 的 SelectedValue 屬性](batch-updating-cs/_static/image12.gif)

**圖 12**： 繫結產品 s `CategoryID` DropDownList s 值`SelectedValue`屬性


一個過去的問題會維持： 如果有產品規定 t`CategoryID`值指定，則資料繫結陳述式上`SelectedValue`會導致例外狀況。 這是因為 DropDownList 包含類別的項目，不提供的產品選項`NULL`資料庫值`CategoryID`。 若要補救這種情況，將 DropDownList s`AppendDataBoundItems`屬性`true`並將新的項目加入至 DropDownList 省略`Value`屬性的宣告式語法。 也就是確定`Categories`DropDownList s 宣告式語法看起來如下：


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

請注意如何`<asp:ListItem Value="">`-選取一個-有其`Value`屬性明確設定為空字串。 回頭參考[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)為何需要這個額外的 DropDownList 項目用來處理的更完整討論的教學課程，`NULL`案例和原因指派`Value`屬性為空字串是不可或缺的。

> [!NOTE]
> 沒有潛在效能和延展性問題值得一提。 因為每個資料列都有使用 DropDownList`CategoriesDataSource`當做其資料來源，`CategoriesBLL`類別 s`GetCategories`方法會呼叫 *n* 瀏覽每個分頁的時間，其中 *n*在 GridView 的資料列數。 這些 *n* 呼叫`GetCategories`導致 *n* 向資料庫查詢。 這樣的影響力，在資料庫上無法減少快取傳回的類別目錄要求每個快取中或透過使用快取相依性或非常短的時間為基礎的到期 SQL 快取層。 如需有關每個要求快取選項，請參閱[`HttpContext.Items`每秒要求的快取存放區](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx)。


## <a name="step-4-completing-the-editing-interface"></a>步驟 4： 完成編輯介面

我們 ve 範本所做的變更數目 GridView s 停頓檢視進度。 請花一點時間來檢視進度透過瀏覽器。 如圖 13 所示，每個資料列使用呈現其`ItemTemplate`，其中包含儲存格 s 中的編輯介面。


[![每個 GridView 資料列是編輯](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**圖 13**： 每個 GridView 資料列會編輯 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image22.png))


有幾個次要的格式化問題，我們應該會負責處理這個時候。 首先，請注意，`UnitPrice`值包含四個小數位數。 若要修正此問題，傳回到`UnitPrice`TemplateField 的`ItemTemplate`和文字方塊 s 智慧標籤，按一下 編輯資料繫結連結。 接下來，指定`Text`應為數字格式屬性。


![格式化的數字的文字屬性](batch-updating-cs/_static/image14.gif)

**圖 14**： 格式`Text`的數字屬性


第二，讓 s 置中的核取方塊`Discontinued`資料行 （而不需要它是靠左對齊）。 按一下 編輯資料行中，從 GridView s 智慧標籤，然後選取`Discontinued`TemplateField 從左下角中的欄位清單。 向下鑽研至`ItemStyle`並設定`HorizontalAlign`Center 圖 15 所示的屬性。


![置中已停止的核取方塊](batch-updating-cs/_static/image15.gif)

**圖 15**: Center`Discontinued`核取方塊


接下來，ValidationSummary 控制項加入網頁，並設定其`ShowMessageBox`屬性`true`及其`ShowSummary`屬性`false`。 也新增按鈕 Web 控制項，在按下時，會更新使用者的變更。 具體來說，新增兩個按鈕 Web 控制項，一個 GridView 上方，另一個下方，設定兩個控制項`Text`更新產品的屬性。

GridView s 自編輯介面定義在其 TemplateFields `ItemTemplate` s， `EditItemTemplate` s 是多餘的而且可能遭到刪除。

進行上述提到格式變更之後，將按鈕控制項，加入和移除不必要`EditItemTemplate`s，您的頁面 s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

圖 16 顯示此頁面已加入按鈕 Web 控制項之後，透過瀏覽器檢視時，以及格式所做的變更。


[![頁面現在包含兩個產品更新 按鈕](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**圖 16**: 頁面現在包含兩個更新產品按鈕 ([按一下以檢視完整大小的影像](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>步驟 5： 更新產品

當使用者造訪此頁他們進行其的修改，然後按一下其中一個的兩個更新產品按鈕。 此時，我們需要以某種方式儲存每個資料列插入的使用者輸入值`ProductsDataTable`執行個體，然後將它傳遞至 BLL 方法會再將之`ProductsDataTable`DAL s 的執行個體`UpdateWithTransaction`方法。 `UpdateWithTransaction`方法，我們在建立[前述教學課程](wrapping-database-modifications-within-a-transaction-cs.md)，可確保批次的變更將會更新以不可部分完成的作業。

建立名為方法`BatchUpdate`中`BatchUpdate.aspx.cs`並加入下列程式碼：


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

這個方法一開始會取得所有產品回`ProductsDataTable`透過呼叫 BLL 的`GetProducts`方法。 接著它會列舉`ProductGrid`GridView s [ `Rows`集合](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)。 `Rows`集合包含[`GridViewRow`執行個體](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridviewrow.aspx)GridView 中顯示每一個資料列。 因為我們會顯示最多十個資料列每個分頁，GridView 的`Rows`集合會有十個以上的項目。

每個資料列`ProductID`捕捉從`DataKeys`集合和適當`ProductsRow`選取從`ProductsDataTable`。 以程式設計方式參考的四個 TemplateField 輸入的控制項，且其值指派給`ProductsRow`執行個體內容。 每個 GridView 之後列值 s 已用來更新`ProductsDataTable`，它 s 傳遞至 BLL s`UpdateWithTransaction`方法，如我們所見前述教學課程中，只會呼叫向下到 DAL 的`UpdateWithTransaction`方法。

本教學課程使用的批次更新演算法更新中的每個資料列`ProductsDataTable`對應至 GridView，不論是否已變更的產品的資訊中的資料列。 這類 blind 更新通常並不是效能問題，而它們可能會導致過多記錄如果作為稽核您變更至資料庫資料表。 回到[執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)教學課程中我們探索批次更新 DataList 介面，加入程式碼，只會更新使用者實際修改這些記錄。 歡迎使用從技術[執行批次更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)如有需要，在本教學課程中，更新程式碼。

> [!NOTE]
> 當繫結透過其智慧標籤在 GridView 的資料來源，Visual Studio 自動將資料來源 s 主要值指派給 GridView 的`DataKeyNames`屬性。 如果您未繫結 ObjectDataSource 透過 GridView s 智慧標籤在 GridView 中步驟 1 所述，則您必須手動設定 GridView s`DataKeyNames`屬性才能存取 ProductID`ProductID`透過每個資料列的值`DataKeys`集合。


使用中的程式碼`BatchUpdate`類似於用於 BLL s`UpdateProduct`方法、 主要的差異在於，在`UpdateProduct`方法只會有一個`ProductRow`從架構擷取執行個體。 指派的屬性的程式碼`ProductRow`之間相同`UpdateProducts`方法和中的程式碼`foreach`迴圈`BatchUpdate`，如整體模式。

若要完成本教學課程中，我們必須將`BatchUpdate`時叫用方法的更新產品按鈕按下。 建立事件處理常式`Click`事件的這兩個按鈕控制項和事件處理常式中加入下列程式碼：


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

第一次進行呼叫以`BatchUpdate`。 下一步`ClientScript property`用來插入將會顯示讀取已更新之產品的 messagebox 的 JavaScript。

花一分鐘，若要測試這段程式碼。 請瀏覽`BatchUpdate.aspx`透過瀏覽器中，編輯資料列數目，並按一下其中一個更新產品按鈕。 假設沒有任何輸入的驗證錯誤，您應該會看到讀取已更新之產品的 messagebox。 若要確認不可部分完成的更新，請考慮加入的隨機`CHECK`條件約束，例如不允許的其中一個`UnitPrice`1234.56 的值。 然後從`BatchUpdate.aspx`，編輯的記錄數，務必要設定其中一個產品的`UnitPrice`禁止的值 (1234.56) 的值。 這應該與其他變更的更新產品按一下該批次作業期間復原為其原始值時產生錯誤。

## <a name="an-alternativebatchupdatemethod"></a>替代`BatchUpdate`方法

`BatchUpdate`我們剛的方法檢查擷取*所有*BLL s 從產品的`GetProducts`方法，然後更新只會出現在 GridView 的記錄。 如果 GridView 不會使用分頁，但如果是的話，可能有數百、 千或數以萬計的產品，但是在 GridView 中的只有 10 個資料列，這個方法很理想。 在這種情況下，取得所有產品資料庫時，才能修改其中 10 個為小於理想。

這些類型的情況中，請考慮使用下列`BatchUpdateAlternate`方法改為：


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate`藉由建立新的空白啟動`ProductsDataTable`名為`products`。 接著，它會逐一 GridView s`Rows`集合，在每個資料列取得特定產品資訊使用 BLL 的`GetProductByProductID(productID)`方法。 擷取`ProductsRow`執行個體有以相同的方式，以更新其屬性`BatchUpdate`，但在更新匯入的資料列之後`products``ProductsDataTable`經由 DataTable s [ `ImportRow(DataRow)`方法](https://msdn.microsoft.com/en-us/library/system.data.datatable.importrow(VS.80).aspx)。

之後`foreach`迴圈完成時，`products`包含一個`ProductsRow`GridView 中每個資料列的執行個體。 由於每個的`ProductsRow`執行個體已新增至`products`（而不是更新），如果我們盲目地將它傳遞給`UpdateWithTransaction`方法`ProductsTableAdatper`會嘗試每筆記錄插入資料庫。 相反地，我們需要指定這些資料列的每個已修改的 （不會新增）。

這可以透過將新方法加入至名為 BLL `UpdateProductsWithTransaction`。 `UpdateProductsWithTransaction`如下所示，設定`RowState`的每個`ProductsRow`中執行個體`ProductsDataTable`至`Modified`然後將傳遞`ProductsDataTable`DAL s`UpdateWithTransaction`方法。


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>總結

在 GridView 提供內建的每一列編輯功能，但不支援建立完整的可編輯的介面。 當我們在本教學課程中所見，這類介面是可行的但需要的工作。 若要建立其中每個資料列都可編輯的 GridView，我們需要將 GridView 的欄位轉換成 TemplateFields，定義中的編輯介面`ItemTemplate`s。 此外，更新所有按鈕 Web 控制項的類型必須新增至頁面上，個別從 GridView。 這些按鈕`Click`事件處理常式必須列舉 GridView s`Rows`集合中，存放區中的變更`ProductsDataTable`，並將更新的資訊傳遞至適當的 BLL 方法。

在下一個教學課程中，我們會看到如何建立批次刪除介面。 特別是，每個 GridView 資料列會包含核取方塊，並而不是更新所有-輸入按鈕，我們必須刪除選取的資料列按鈕。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和 David Suru。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](wrapping-database-modifications-within-a-transaction-cs.md)
[下一頁](batch-deleting-cs.md)
