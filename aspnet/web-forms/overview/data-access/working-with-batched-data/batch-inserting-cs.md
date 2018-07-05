---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: 批次插入 (C#) |Microsoft Docs
author: rick-anderson
description: 了解如何在單一作業中插入多個資料庫的記錄。 在使用者介面層，我們會擴充以允許使用者輸入多個 n GridView...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 9979a991935d97ef7c3b2ac62666da318b95d063
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829055"
---
<a name="batch-inserting-c"></a>批次插入 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip)或[下載 PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> 了解如何在單一作業中插入多個資料庫的記錄。 在使用者介面層，我們會擴充 GridView，以允許使用者輸入多個新的記錄。 資料存取層中，我們換行以確保所有插入作業成功，或所有插入作業會回復在交易內的多個 Insert 作業的內容。


## <a name="introduction"></a>簡介

在 [批次更新](batch-updating-cs.md)教學課程中我們探討了自訂 GridView 控制項來呈現可編輯多筆記錄所在的介面。 瀏覽頁面的使用者無法進行一系列的變更，然後，使用單一按鈕點選，執行批次更新。 針對使用者經常更新一次的多筆記錄的情況下，這類介面可以節省無數的按下滑鼠和鍵盤-滑鼠內容切換，相較於預設每個資料列編輯功能，已先瀏覽回到[插入、 更新和刪除資料的概觀](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教學課程。

新增記錄時，也可以套用這個概念。 想像一下，這裡 Northwind traders 我們常收到寄送包含許多特定類別的產品的供應商。 例如，我們可能會收到東京 Traders 六個不同的茶和 coffee 產品的出貨。 如果使用者輸入六個產品一次透過 DetailsView 控制項，它們必須不斷地選擇許多相同的值： 他們需要選擇相同的類別目錄 （飲料），相同的供應商 (東京 Traders)、 已停止的相同值 （False)，和相同的單位，順序值 (0)。 這個重複的資料項目不只會耗費時間，但很容易發生錯誤。

我們可以很輕鬆地建立批次插入介面，可讓使用者選擇的供應商和類別目錄一次，輸入一系列的產品名稱和單價，，然後按一下 加入資料庫中的新產品的按鈕 （請參閱 圖 1）。 增加每個產品，其`ProductName`並`UnitPrice`資料欄位指派的文字方塊中輸入的值時其`CategoryID`和`SupplierID`值從在最上層的表單 fo dropdownlist 進行指派的值。 `Discontinued`並`UnitsOnOrder`值都會設為硬式編碼值`false`和 0，分別。


[![批次插入介面](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**圖 1**: 批次插入介面 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image3.png))


在本教學課程中，我們將建立會實作批次插入介面 [圖 1] 所示的頁面。 為先前的兩個教學課程中，我們會確保不可部分完成交易範圍內包裝插入。 讓 s 開始 ！

## <a name="step-1-creating-the-display-interface"></a>步驟 1： 建立顯示介面

本教學課程會組成的單一頁面分為兩個區域： 顯示區域和插入的區域。 顯示介面，我們將建立在此步驟中，在 GridView 中顯示的產品，並包含標題為處理程序產品出貨的按鈕。 按一下此按鈕時，就會顯示介面取代插入介面後，圖 1 所示。 顯示介面傳回之後新增的產品出貨，或按一下 [取消] 按鈕。 我們將在步驟 2 中建立的插入的介面。

當建立頁面有兩個介面，其中只有一個會顯示一次，每個介面通常會放置內[面板 Web 控制項](http://www.w3schools.com/aspnet/control_panel.asp)，做為其他控制項的容器。 因此，我們的頁面上會有兩個面板控制項一個針對每個介面。

首先開啟`BatchInsert.aspx`頁面中`BatchData`資料夾，然後從 工具箱 拖曳至設計工具拖曳面板 （請參閱 圖 2）。 設定面板 s`ID`屬性設`DisplayInterface`。 當將面板加入至設計工具中，其`Height`和`Width`屬性分別設定為 50px 和 125px。 清除這些屬性值，從 [屬性] 視窗。


[![從 [工具箱] 拖曳至設計工具拖曳面板](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**圖 2**： 從 [工具箱] 拖曳至設計工具拖曳面板 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image6.png))


接下來，將按鈕和 GridView 控制項拖曳到 [面板] 中。 設定 [s] 按鈕`ID`屬性，以`ProcessShipment`及其`Text`程序產品出貨的屬性。 設定 GridView s`ID`屬性，以`ProductsGrid`和它的智慧標籤，從繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定提取其資料從 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 因為此 GridView 只用於顯示資料，設定下拉式清單中更新、 插入和刪除為 [（無）] 索引標籤。 按一下 完成 以完成設定資料來源精靈。


[![顯示從 ProductsBLL 類別的 GetProducts 方法傳回的資料](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**圖 3**： 顯示從傳回的資料`ProductsBLL`類別 s`GetProducts`方法 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image9.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**圖 4**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image12.png))


完成 ObjectDataSource 精靈之後，Visual Studio 會將 BoundFields 及其產品資料欄位。 移除以外的所有`ProductName`， `CategoryName`， `SupplierName`， `UnitPrice`，和`Discontinued`欄位。 隨意美觀的任何自訂。 我決定來格式化`UnitPrice`欄位為貨幣值時，重新排列欄位，並重新命名數個欄位`HeaderText`值。 也設定 GridView，以包含分頁和排序支援，藉由檢查 GridView s 智慧標籤啟用分頁，並按一下 啟用排序核取方塊。

加入面板、 按鈕、 GridView 和 ObjectDataSource 控制項，並自訂 GridView 的欄位之後, 您頁面 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

請注意，按鈕和 GridView 的標記出現在開頭和結尾`<asp:Panel>`標記。 因為這些控制項全都位於`DisplayInterface`面板中，我們可以隱藏它們只需要設定 [s] 面板`Visible`屬性設`false`。 步驟 3 會查看以程式設計方式變更面板的`Visible`屬性，以回應按鈕按一下以顯示一個介面，同時隱藏其他。

請花一點時間檢閱我們透過瀏覽器的進度。 如 圖 5 所示，您應該會看到上述的 GridView 會列出產品 10 次的程序產品出貨 按鈕。


[![GridView 列出產品，並提供排序和分頁功能](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**圖 5**: GridView 列出的產品和提供排序和分頁功能 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>步驟 2： 建立插入介面

顯示介面完成後，我們準備好建立插入的介面。 本教學課程中，讓建立插入介面提示您輸入單一的供應商和類別值，然後可讓使用者輸入最多五個產品名稱和單位價格值。 使用這個介面，使用者可以新增一到五個新產品的所有共用相同的類別和供應商，但擁有唯一的產品名稱和價格。

開始從工具箱拖曳至設計工具中，將它放在下方現有拖曳面板`DisplayInterface`面板。 設定`ID`屬性的新加入的面板`InsertingInterface`並設定其`Visible`屬性設`false`。 我們將新增程式碼，以設定`InsertingInterface`面板 s`Visible`屬性設`true`在步驟 3 中。 也會清除面板 s`Height`和`Width`屬性值。

接下來，我們需要建立可在 圖 1 所示的插入介面。 您可以透過各種不同的 HTML 的技術，建立此介面，但我們將使用一個相當簡單： 四個資料行中，七個資料列的資料表。

> [!NOTE]
> 輸入標記的 HTML 時`<table>`項目，我想要使用的來源檢視。 雖然 Visual Studio 沒有新增的工具`<table>`透過設計工具的項目，設計工具似乎所有太願意將如蠢`style`到標記的設定。 當我建立`<table>`標記中，我通常返回設計工具來新增 Web 控制項，並設定其屬性。 建立具有預先決定的資料行和資料列的資料表時我比較喜歡用靜態 HTML 而非[資料表 Web 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx)因為任何放置在資料表 Web 控制項內的 Web 控制項只能存取使用`FindControl("controlID")`模式。 我，不過，使用資料表 Web 控制項的動態調整大小的資料表 （而是其資料列或資料行根據部分資料庫或使用者指定的準則），因為資料表 Web 控制項可以用來以程式設計方式建構。


輸入下列標記內`<asp:Panel>`標記`InsertingInterface`面板：


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

這`<table>`標記不包含任何 Web 控制項，但我們會將這些新增立刻顯示。 請注意，每個`<tr>`項目包含特定的 CSS 類別設定：`BatchInsertHeaderRow`標頭資料列的供應商和類別目錄 dropdownlist 進行位置;`BatchInsertFooterRow`新增的產品出貨和取消按鈕位置; 頁尾資料列和替代`BatchInsertRow`和`BatchInsertAlternatingRow`值的資料列將包含產品和單位價格文字方塊控制項。 我已建立對應的 CSS 類別，在`Styles.css`檔案，以提供插入介面外觀類似的 GridView 和 DetailsView 控制項我們已在整個這些教學課程中使用。 這些的 CSS 類別如下所示。


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

此標記中，輸入，以返回 [設計] 檢視。 這`<table>`應該要顯示的四個資料行中，七個資料列的資料表，在設計師中，如 圖 6 所示。


[![插入介面由所組成的四資料行，七個資料列的資料表](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**圖 6**: 插入介面由所組成的四資料行，七個資料列的資料表 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image18.png))


我們重新現在已準備好將 Web 控制項新增至插入的介面。 從 [工具箱] 將兩個 dropdownlist 進行拖曳表其中一個用於供應商，一個用於類別目錄中適當的資料格。

設定供應商 DropDownList s`ID`屬性，以`Suppliers`並將它繫結至名為新 ObjectDataSource `SuppliersDataSource`。 設定要擷取其資料的新 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers`方法，並設定更新索引標籤為 （無） s 下拉式清單。 按一下 完成 以完成精靈。


[![設定為使用 SuppliersBLL 類別的 GetSuppliers 方法的 ObjectDataSource](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**圖 7**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 s`GetSuppliers`方法 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image21.png))


已`Suppliers`DropDownList 顯示器`CompanyName`資料欄位並使用`SupplierID`資料欄位做為其`ListItem`的值。


[![顯示 供應商資料欄位和 SupplierID 做為值](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**圖 8**： 顯示`CompanyName`資料欄位並使用`SupplierID`做為值 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image24.png))


命名第二個 DropDownList`Categories`並將它繫結至名為新 ObjectDataSource `CategoriesDataSource`。 設定`CategoriesDataSource`若要使用的 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法; 設定下拉式清單會列出在 更新和刪除的索引標籤，為 （無），然後按一下 完成 以完成精靈。 最後，有 DropDownList 顯示器`CategoryName`資料欄位並使用`CategoryID`做為值。

已新增並繫結至適當地設定 ObjectDataSources 這些兩個 dropdownlist 進行之後，您的畫面看起來應該類似於 圖 9。


[![標頭資料列現在包含供應商和類別 dropdownlist 進行](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**圖 9**: 標頭資料列現在包含`Suppliers`並`Categories`dropdownlist 進行 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image27.png))


我們現在需要建立文字方塊來收集每個新的產品價格與名稱。 將 TextBox 控制項從 [工具箱] 拖曳至設計工具拖曳每五個產品名稱和價格資料列。 設定`ID`屬性的文字方塊來`ProductName1`， `UnitPrice1`， `ProductName2`， `UnitPrice2`， `ProductName3`， `UnitPrice3`，依此類推。

在每個單位價格文字方塊中，設定新增 CompareValidator`ControlToValidate`屬性為適當`ID`。 Ssprop_param_table_default`Operator`屬性，以`GreaterThanEqual`，`ValueToCompare`為 0，和`Type`到`Currency`。 這些設定會指示 CompareValidator，以確保價格中，如果輸入有效的貨幣值大於或等於零，這個值。 設定`Text`屬性，以\*，和`ErrorMessage`價格必須大於或等於零。 此外，請省略任何貨幣符號。

> [!NOTE]
> 插入介面並未包含任何 RequiredFieldValidator 控制項，即使`ProductName`欄位中`Products`資料庫資料表不允許`NULL`值。 這是因為我們想要讓使用者輸入最多五個產品。 比方說，如果使用者提供的產品名稱和單位價格的前三個資料列，將最後兩個資料列留為空白，我們的 d 加入三個新的產品系統。 因為`ProductName`是必要，不過，我們必須以程式設計方式檢查，確保當單價輸入，提供對應的產品名稱值。 我們將會處理在步驟 4 中的這項檢查。


當驗證的使用者的輸入，CompareValidator 會報告無效的資料，如果值包含貨幣符號。 新增 $ 前面每單位價格做為視覺提示，指示使用者輸入價格時省略的貨幣符號的文字方塊。

最後，將 ValidationSummary 控制項內新增`InsertingInterface`面板中，設定其`ShowMessageBox`屬性設`true`及其`ShowSummary`屬性設`false`。 使用這些設定，如果使用者輸入無效的單位價格值，星號就會顯示違規的 TextBox 控制項旁邊，ValidationSummary 會顯示用戶端訊息方塊會顯示我們稍早指定的錯誤訊息。

此時，您的畫面應該看起來類似 圖 10。


[![插入介面現在包含文字方塊的產品名稱和價格](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**圖 10**: 插入介面現在包含文字方塊的產品名稱和價格 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image30.png))


接下來我們要新增的產品出貨和取消按鈕加入頁尾資料列。 將按鈕拖曳兩個控制項從 [工具箱] 的頁尾插入介面中，設定按鈕`ID`屬性，以`AddProducts`並`CancelButton`和`Text`產品出貨與 [取消]，將它分別加入的屬性。 此外，設定`CancelButton`控制 s`CausesValidation`屬性設`false`。

最後，我們需要新增的標籤 Web 控制項，將會顯示兩個介面的狀態訊息。 比方說，當使用者成功地將新的出貨的產品，我們想要傳回以顯示介面，並顯示確認訊息。 如果，不過，使用者提供 新的產品，但結束的產品名稱的價格，我們要顯示的警告訊息，因為`ProductName`是必填欄位。 因為我們需要針對這兩個介面顯示此訊息，請將它放在外部面板頁面頂端。

標籤 Web 控制項從工具箱拖曳至設計工具中頁面的頂端。 設定`ID`屬性，以`StatusLabel`，清除出`Text`屬性，並設定`Visible`並`EnableViewState`屬性，以`false`。 如我們所見先前教學課程中，設定`EnableViewState`屬性設`false`讓我們能夠以程式設計方式變更標籤的屬性值，並讓它們自動還原成其預設值，在後續的回傳。 這可簡化的程式碼顯示狀態訊息，以在後續的回傳會消失的某些使用者動作的回應。 最後，設定`StatusLabel`控制 s`CssClass`為 [警告] 的 CSS 類別名稱的屬性定義於`Styles.css`會以大型、 斜體、 粗體、 紅色字型顯示的文字。

圖 11 顯示 Visual Studio 設計工具，標籤已新增並設定之後。


[![將 statuslabel 設控制項在兩個面板控制項上方放置](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**圖 11**： 的地方`StatusLabel`控制上述兩個面板控制項 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>步驟 3： 顯示之間切換，並插入介面

現在我們已完成我們的顯示和插入介面，但我們重新仍處於具有兩個工作的標記：

- 切換顯示和插入介面
- 加入到資料庫次出貨的產品

目前，顯示介面會顯示，但是隱藏插入的介面。 這是因為`DisplayInterface`面板 s`Visible`屬性設定為`true`（預設值），而`InsertingInterface`面板 s`Visible`屬性設定為`false`。 我們只需要將切換 s 的每個控制項的兩個介面之間切換`Visible`屬性值。

我們想要按一下處理程序產品出貨按鈕時，從顯示的介面移至插入的介面。 因此，建立事件處理常式，此按鈕 s`Click`事件，其中包含下列程式碼：


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

此程式碼只會隱藏`DisplayInterface`面板，並顯示`InsertingInterface`面板。

插入介面中的出貨和 [取消] 按鈕控制項中的新增產品，接下來，建立事件處理常式。 按一下任一個按鈕時，我們需要回復成顯示介面。 建立`Click`兩個事件處理常式按鈕控制項，好讓它們呼叫`ReturnToDisplayInterface`，我們會暫時新增的方法。 除了隱藏`InsertingInterface`面板，並顯示`DisplayInterface` 面板中，`ReturnToDisplayInterface`方法需要傳回 Web 控制項為其預先編輯的狀態。 這需要設定 dropdownlist 進行`SelectedIndex`屬性為 0，而且清除`Text`文字方塊控制項的屬性。

> [!NOTE]
> 請考慮可能會發生什麼事如果我們嘛 t 回復到其預先編輯狀態控制項，然後再回到 顯示介面。 使用者可能按一下處理程序產品出貨] 按鈕，輸入的產品出貨，，然後按一下 [新增出貨的產品。 這會將產品加入，並返回顯示介面中的使用者。 此時使用者可能會想要新增另一個出貨。 按一下 [處理程序產品出貨] 按鈕就會傳回插入的介面，但 DropDownList 選取項目和文字方塊值會仍填入其先前的值。


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

兩者`Click`只要呼叫事件處理常式`ReturnToDisplayInterface`方法，雖然我們會返回新增的產品出貨從`Click`事件處理常式，在步驟 4，並加入程式碼來儲存產品。 `ReturnToDisplayInterface` 一開始會傳回`Suppliers`和`Categories`dropdownlist 進行其第一個選項。 兩個常數`firstControlID`並`lastControlID`標記的起始和結束用來命名的產品名稱和單位價格中插入文字方塊介面，並使用的界限內的控制項索引值`for`設定的迴圈`Text`文字方塊控制項的屬性後為空字串。 最後，面板`Visible`屬性重設，以便隱藏插入的介面和顯示介面所示。

請花一點時間來測試此頁面在瀏覽器中。 第一次造訪網頁時應該會看到顯示介面，如 [圖 5] 顯示中。 按一下 [處理程序產品出貨] 按鈕。 頁面會回傳，您現在應該看到插入的介面，如 圖 12 所示。 按一下任一個新增的產品出貨 或 取消 按鈕返回顯示介面。

> [!NOTE]
> 檢視時插入的介面，請花一點時間來測試 CompareValidators 單價的文字方塊上。 您應該會看到用戶端 messagebox 時按一下 新增產品出貨按鈕具有無效的貨幣值或值小於零的價格從出現的警告。


[![按一下處理程序的產品出貨按鈕後顯示插入介面](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**圖 12**： 按一下處理程序的產品出貨按鈕後顯示的插入介面 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>步驟 4： 加入產品

全部保留本教學課程是要新增的產品中的資料庫中儲存的產品出貨按鈕 s`Click`事件處理常式。 這可藉由建立`ProductsDataTable`並新增`ProductsRow`提供的產品名稱的每個執行個體。 一次這些`ProductsRow`已加入，我們將呼叫`ProductsBLL`類別 s`UpdateWithTransaction`方法並傳入`ProductsDataTable`。 請記得，`UpdateWithTransaction`方法，以建立回到[資料庫修改包裝在交易](wrapping-database-modifications-within-a-transaction-cs.md)教學課程中，傳遞`ProductsDataTable`來`ProductsTableAdapter`s`UpdateWithTransaction`方法。 從該處啟動 ADO.NET 交易和 TableAdatper 問題`INSERT`陳述式之資料庫的每個已加入`ProductsRow`DataTable 中。 假設所有產品會都加入不會發生錯誤，就會認可交易，否則它會回復。

出貨按鈕 s 中的新增產品的程式碼`Click`事件處理常式也需要執行一些錯誤檢查。 因為沒有用於插入介面沒有 RequiredFieldValidators，使用者無法輸入價格的產品時省略其名稱。 因為產品的名稱是必要的如果事件發生這種情況我們需要提醒使用者並不會進行插入。 完整`Click`事件處理常式程式碼如下：


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

事件處理常式一開始會確保`Page.IsValid`屬性傳回的值`true`。 如果它傳回`false`，然後這表示一個或多個 CompareValidators 報告無效的資料; 在此情況下我們不想要嘗試插入輸入的產品或就會出現的例外狀況時嘗試指派的使用者輸入的單價值`ProductsRow`s`UnitPrice`屬性。

接下來，新`ProductsDataTable`會建立執行個體 (`products`)。 A`for`迴圈來逐一查看的產品名稱和單位價格的文字方塊和`Text`屬性會讀取至區域變數`productName`和`unitPrice`。 如果使用者輸入值的單位價格但沒有對應的產品名稱，`StatusLabel`顯示訊息，如果您提供一個單位價格，您也必須包含的產品名稱和結束事件處理常式。

如果未提供的產品名稱，新`ProductsRow`執行個體使用建立`ProductsDataTable`s`NewProductsRow`方法。 這個新`ProductsRow`執行個體 s`ProductName`屬性設定為目前的產品名稱時的文字方塊`SupplierID`並`CategoryID`屬性指派給`SelectedValue`dropdownlist 進行插入介面 s 標頭中的屬性。 如果使用者輸入的產品的價格的值，將它指派給`ProductsRow`執行個體 s`UnitPrice`屬性; 否則屬性是左側未指定，這會導致`NULL`值`UnitPrice`資料庫中。 最後，`Discontinued`並`UnitsOnOrder`屬性會指派為硬式編碼值`false`和 0，分別。

屬性已指派給之後`ProductsRow`執行個體，它會新增至`ProductsDataTable`。

在完成`for`迴圈中，我們會檢查是否已新增的任何產品。 使用者可能在所有情況下，按一下新增的產品出貨之前輸入的任何產品名稱或價格。 如果沒有至少一個產品中的`ProductsDataTable`，則`ProductsBLL`類別的`UpdateWithTransaction`呼叫方法。 接下來，資料會重新繫結至`ProductsGrid`GridView，因此新加入的產品會出現在顯示介面。 `StatusLabel`更新為顯示確認訊息和`ReturnToDisplayInterface`叫用時，隱藏插入介面和顯示顯示介面。

如果不輸入任何產品，插入介面仍然能夠保持顯示但沒有產品已新增的訊息。 請輸入產品名稱，並顯示於文字方塊中的單位價格。

圖 13、 14 及 15 顯示插入和顯示作用中的介面。 [圖 13] 中的使用者輸入不含對應的產品名稱的單位價格值。 圖 14 顯示顯示介面之後三個新產品已經成功加入，而 圖 15 所顯示的是兩個新加入的產品在 gridview 裡 （在前一個頁面是第三個）。


[![產品名稱是必要時輸入單位價格](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**圖 13**: 產品名稱是必要時輸入單位價格 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image39.png))


[![已新增三個新 Veggies 供應商 Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**圖 14**： 三個新 Veggies 已新增供應商 Mayumi s ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image42.png))


[![新的產品可在 GridView 的最後一頁](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**圖 15**: 新產品可在 GridView 的最後一頁 ([按一下以檢視完整大小的影像](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> 批次插入此教學課程中使用的邏輯包裝插入的交易範圍內。 若要確認，故意放入資料庫層級錯誤。 比方說，而不是指派的新`ProductsRow`執行個體 s`CategoryID`屬性設為所選的值，在`Categories`下拉式清單中，指派其值，例如`i * 5`。 這裡`i`迴圈索引子，且具有範圍從 1 到 5 的值。 因此，當加入批次中的兩個或多個產品放入第一次的產品必須是有效`CategoryID`值 (5)，但後續的產品會有`CategoryID`最多不相符的值`CategoryID`中的值`Categories`資料表。 結果是，在第一個`INSERT`會成功，後續的項目將會失敗與外部索引鍵條件約束違規。 由於是不可部分完成的批次插入第一個`INSERT`將會回復，傳回其之前的批次插入程序的狀態資料庫開始。


## <a name="summary"></a>總結

這和先前的兩個教學課程中，我們已建立介面，允許更新、 刪除和插入的資料批次，全部都使用我們新增資料存取層中的交易支援[包裝資料庫修改在交易內](wrapping-database-modifications-within-a-transaction-cs.md)教學課程。 某些情況下，這類批次的處理使用者介面大幅提升終端使用者的效率由縮減下數按、 回傳和鍵盤-滑鼠內容切換，同時維持資料完整性的基礎。

本教學課程中完成我們看看使用批次的資料。 教學課程的下一組探索各種進階的資料存取層案例，包括使用預存程序在 TableAdapter 的方法中，連接和命令層級設定在 DAL、 加密的連接字串和更多功能 ！

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 導致檢閱者針對本教學課程已 Hilton giesenow 將和 S ren Jacob Lauritsen。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](batch-deleting-cs.md)
> [下一頁](wrapping-database-modifications-within-a-transaction-vb.md)
