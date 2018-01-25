---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: "批次插入 (VB) |Microsoft 文件"
author: rick-anderson
description: "了解如何在單一作業中插入多個資料庫的記錄。 使用者介面層級中，我們會延伸以允許使用者輸入多個 n GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f09b5fd86c1cc6641fb42a466b07da161c1dd35
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="batch-inserting-vb"></a>批次插入 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip)或[下載 PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> 了解如何在單一作業中插入多個資料庫的記錄。 在使用者介面層我們會擴充 GridView 允許使用者輸入多個新的記錄。 資料存取層中我們會換行以確保所有插入會都成功，或所有的插入將會回復在交易內的多個 Insert 作業。


## <a name="introduction"></a>簡介

在[批次更新](batch-updating-vb.md)教學課程中我們探討了自訂 GridView 控制項呈現的介面已可編輯多筆記錄的位置。 瀏覽頁面的使用者，可以進行一系列的變更，然後按一下單一按鈕，執行批次更新。 使用者經常更新一次的多筆記錄的情況下，這類介面可以將儲存無數點選，相較於預設的鍵盤-滑鼠內容切換每個資料列第一次瀏覽回到的編輯功能[插入、 更新和刪除資料的概觀](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程。

新增記錄時，也可以套用這個概念。 想像一下，這裡 Northwind traders 我們經常收到出貨包含數字的特定某類產品的供應商。 例如，我們可能會收到東京 Traders 六個不同茶杯咖啡產品的出貨。 如果使用者透過 DetailsView 控制項一次輸入一個的六個產品，則必須不斷地選擇許多相同的值： 需要選擇相同類別目錄 （飲料） 相同的供應商 (東京 Traders)、 已停止的相同值 （預設值），和相同的單位，順序值 (0)。 重複項目不僅耗費時間，且容易發生錯誤。

我們可以輕鬆地建立批次插入介面，可讓使用者選擇的供應商和類別目錄一次，輸入一系列的產品名稱和單位價格，然後按一下按鈕，將新的產品加入至資料庫 （請參閱圖 1）。 增加每個產品，其`ProductName`和`UnitPrice`指派資料欄位的文字方塊中輸入的值時其`CategoryID`和`SupplierID`值在最上層的表單 fo DropDownLists 從指派的值。 `Discontinued`和`UnitsOnOrder`值都會設為硬式編碼值`False`和 0 分別。


[![批次插入介面](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**圖 1**： 批次插入介面 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image3.png))


在本教學課程中，我們將建立會實作批次插入介面中圖 1 顯示的頁面。 做為與先前的兩個教學課程，我們會自動換行插入的交易以確認不可部分完成範圍內。 Let s 開始 ！

## <a name="step-1-creating-the-display-interface"></a>步驟 1： 建立顯示介面

本教學課程所組成的單一頁面，就會劃分為兩個區域： 顯示區域和可插入的區域。 顯示介面中，我們將建立在此步驟中，在 GridView 中顯示的產品，並包含標題為處理程序的產品出貨的按鈕。 按一下此按鈕時，顯示介面會取代插入介面，圖 1 所示。 顯示介面傳回之後新增的產品出貨，或按一下 [取消] 按鈕。 我們將在步驟 2 中建立插入介面。

當建立頁面有兩個介面，其中只有一個會顯示一次，每個介面通常會放置內[面板 Web 控制項](http://www.w3schools.com/aspnet/control_panel.asp)，做為其他控制項的容器。 因此，我們的頁面將有兩個面板控制項一，每個介面。

先開啟`BatchInsert.aspx`頁面`BatchData`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳到面板 （請參閱圖 2）。 設定面板 s`ID`屬性`DisplayInterface`。 將面板加入至設計工具中，當其`Height`和`Width`屬性分別設定為 50px 和 125px。 清除這些屬性值，從 [屬性] 視窗。


[![從 [工具箱] 拖曳至設計工具拖曳到面板](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**圖 2**： 從 [工具箱] 拖曳至設計工具拖曳到面板 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image6.png))


接下來，將按鈕和 GridView 控制項拖曳到 [面板]。 設定按鈕 s`ID`屬性`ProcessShipment`及其`Text`程序的產品出貨的屬性。 設定 GridView s`ID`屬性`ProductsGrid`和其智慧標籤上，從繫結到名為新 ObjectDataSource `ProductsDataSource`。 設定提取資料的來源 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 此 GridView 只用於顯示資料，因為設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。 按一下 完成 來完成設定資料來源精靈。


[![顯示從 ProductsBLL 類別的 GetProducts 方法傳回的資料](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**圖 3**： 顯示從傳回的資料`ProductsBLL`類別 s`GetProducts`方法 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image9.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**圖 4**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image12.png))


完成 ObjectDataSource 精靈之後，Visual Studio 會加入 BoundFields 以及 CheckBoxField 產品資料欄位。 移除以外的所有`ProductName`， `CategoryName`， `SupplierName`， `UnitPrice`，和`Discontinued`欄位。 請隨意進行任何美觀的自訂。 我決定格式化`UnitPrice`為貨幣值時，欄位重新排列欄位，並重新命名數個欄位`HeaderText`值。 也設定 GridView 包含分頁和排序支援，藉由檢查 GridView s 智慧標籤中的啟用分頁，以啟用排序核取方塊。

加入面板、 按鈕、 GridView 中和 ObjectDataSource 控制項和自訂 GridView 的欄位之後, 您頁面 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

請注意，按鈕和 GridView 的標記出現在開頭和結尾`<asp:Panel>`標記。 因為這些控制項內`DisplayInterface`面板中，我們可以隱藏它們只需要設定面板 s`Visible`屬性`False`。 步驟 3 會查看以程式設計方式變更面板的`Visible`屬性，以回應按鈕按一下以顯示同時隱藏另一個介面。

請花一點時間來檢視進度透過瀏覽器。 如圖 5 所示，您應該會看到上述列出的產品十一次的 GridView 的程序的產品出貨按鈕。


[![在 GridView 列出的產品，並提供排序和分頁功能](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**圖 5**: GridView 會列出產品和可提供排序和分頁功能 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>步驟 2： 建立插入介面

以顯示介面完成之後，我們準備好建立插入介面。 本教學課程，可讓建立插入介面會提示輸入單一的供應商和類別值，然後可讓使用者輸入最多五個產品名稱和單位價格值。 使用這個介面，使用者可以加入 1 到 5 的新產品，所有共用相同的類別目錄和供應商，但擁有唯一的產品名稱和價格。

開始從工具箱拖曳至設計工具中，將其放在現有拖曳面板`DisplayInterface`面板。 設定`ID`屬性，這個新加入的面板`InsertingInterface`並設定其`Visible`屬性`False`。 我們會將新增程式碼，設定`InsertingInterface`面板 s`Visible`屬性`True`在步驟 3 中。 也會清除面板 s`Height`和`Width`屬性值。

接下來，我們必須建立可在 圖 1 顯示的插入介面。 這個介面可建立透過不同的 HTML 技術，但我們將使用一個相當簡單： 四個資料行中，七個資料列的資料表。

> [!NOTE]
> 當輸入標記的 HTML`<table>`項目，我想要使用的來源檢視。 雖然 Visual Studio 沒有工具加入`<table>`透過設計工具項目，在設計工具似乎所有太願意將騙取如`style`設定的標記。 當我建立`<table>`標記，通常是傳回至設計工具，以加入 Web 控制項，並設定其屬性。 使用預先決定的資料行和資料列中建立資料表時我更傾向於使用靜態 HTML 而[資料表 Web 控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx)因為任何放置在資料表 Web 控制項內的 Web 控制項只能存取使用`FindControl("controlID")`模式。 不過，作業，動態調整大小的資料表 （而其資料列或資料行依據部分資料庫或使用者指定的條件），使用資料表 Web 控制項自資料表 Web 控制項可用於以程式設計方式建構。


輸入下列標記內`<asp:Panel>`標記`InsertingInterface`面板：


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

這`<table>`標記不包含任何 Web 控制項，但我們會將這些新增立刻顯示。 請注意，每個`<tr>`項目包含特定的 CSS 類別設定：`BatchInsertHeaderRow`供應商和類別目錄 DropDownLists 位置; 標頭資料列`BatchInsertFooterRow`新增產品出貨和取消按鈕的位置; 頁尾資料列和替代`BatchInsertRow`和`BatchInsertAlternatingRow`值的資料列將包含產品和單位價格 TextBox 控制項。 我已建立在對應的 CSS 類別`Styles.css`檔案，以提供外觀類似的 GridView 和 DetailsView 插入介面控制我們 ve 這些教學課程中使用。 這些 CSS 類別如下所示。


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

輸入此標記，以返回 [設計] 檢視。 這`<table>`應該要顯示的四個資料行中，七個資料列的資料表，在設計師中，如圖 6 所示。


[![插入介面由所組成的四資料行，七個資料列的資料表](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**圖 6**: 插入介面由所組成的四資料行，七個資料列的資料表 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image18.png))


我們 re 現在已準備好將 Web 控制項加入插入的介面。 從 [工具箱] 將兩個 DropDownLists 拖曳資料表一個供應商，另一個類別目錄中的適當資料格。

設定供應商 DropDownList s`ID`屬性`Suppliers`和繫結到名為新 ObjectDataSource `SuppliersDataSource`。 設定要擷取其資料從新的 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers`方法並將更新索引標籤上 s 為 (None) 的下拉式清單。 按一下 [完成] 5d; 以完成精靈。


[![設定為使用 SuppliersBLL 類別的 GetSuppliers 方法 ObjectDataSource](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**圖 7**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 s`GetSuppliers`方法 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image21.png))


具有`Suppliers`DropDownList 顯示`CompanyName`資料欄位和使用`SupplierID`資料欄位做為其`ListItem`的值。


[![顯示供應商資料欄位，並使用 SupplierID 當做值](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**圖 8**： 顯示`CompanyName`資料欄位和使用`SupplierID`做為值 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image24.png))


將第二個 DropDownList`Categories`和繫結到名為新 ObjectDataSource `CategoriesDataSource`。 設定`CategoriesDataSource`使用 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法; 組下拉式清單中 （無） 來更新和刪除索引標籤，然後按一下 「 完成 」 以完成精靈。 最後，有 DropDownList 顯示`CategoryName`資料欄位和使用`CategoryID`做為值。

這些兩個 DropDownLists 已經加入及繫結至適當地設定 ObjectDataSources 之後，您的畫面看起來應該類似於圖 9。


[![標頭資料列現在包含供應商和類別 DropDownLists](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**圖 9**: 標頭資料列現在包含`Suppliers`和`Categories`DropDownLists ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image27.png))


我們現在必須建立要收集的名稱和每個新的產品價格的文字方塊。 將文字方塊控制項從 [工具箱] 拖曳至設計工具拖曳每五個產品名稱和價格的資料列。 設定`ID`屬性的文字方塊來`ProductName1`， `UnitPrice1`， `ProductName2`， `UnitPrice2`， `ProductName3`， `UnitPrice3`，依此類推。

在每個單位價格等文字方塊中，設定之後新增 CompareValidator`ControlToValidate`屬性為適當`ID`。 也設定`Operator`屬性`GreaterThanEqual`，`ValueToCompare`為 0，和`Type`至`Currency`。 這些設定會指示 CompareValidator，以確保價格，如果輸入，是有效的貨幣值的大於或等於零。 設定`Text`屬性\*，和`ErrorMessage`價格必須大於或等於零。 此外，請省略任何貨幣符號。

> [!NOTE]
> 插入介面並未包含任何 RequiredFieldValidator 控制項，即使`ProductName`欄位`Products`資料庫資料表不允許`NULL`值。 這是因為我們想要讓使用者可以輸入最多五個產品。 例如，如果使用者提供的產品名稱和單位價格的前三個資料列，留下最後兩個資料列空白，我們 d 加入三個新的產品系統。 因為`ProductName`是必要，不過，我們需要以程式設計方式檢查，確保當單位價格輸入，提供對應的產品名稱值。 我們將會處理此檢查在步驟 4。


當驗證的使用者的輸入，CompareValidator 如果值包含貨幣符號，就會報告無效的資料。 將 $ 前面每個單位價格做為視覺提示，指示使用者輸入價格時省略的貨幣符號的文字方塊。

最後，將加入 ValidationSummary 控制項內`InsertingInterface`面板中，設定其`ShowMessageBox`屬性`True`及其`ShowSummary`屬性`False`。 這些設定，如果使用者輸入無效的單位價格的值，違規的 TextBox 控制項旁邊會出現星號，ValidationSummary 會顯示顯示我們稍早指定的錯誤訊息的用戶端 messagebox。

此時，您的畫面看起來應該類似於圖 10。


[![插入介面現在包含文字方塊的產品名稱和價格](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**圖 10**: 插入介面現在包含文字方塊的產品名稱及價格 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image30.png))


接下來我們要新增的產品出貨和 [取消] 按鈕加入頁尾資料列。 將按鈕拖曳兩個控制項從 [工具箱] 插入介面中，頁尾設定按鈕`ID`屬性`AddProducts`和`CancelButton`和`Text`分別新增產品出貨和 [取消]，從屬性。 此外，設定`CancelButton`控制項 s`CausesValidation`屬性`false`。

最後，我們需要加入標籤 Web 控制項，就會顯示兩個介面的狀態訊息。 例如，當使用者已成功將新的產品出貨，我們想要回到顯示介面，並顯示確認訊息。 不過，使用者提供 新的產品，但關閉產品名稱的分葉價格，如果我們需要顯示警告訊息，因為`ProductName`是必填欄位。 因為我們需要兩個介面顯示這個訊息，請將它放在外部面板頁面的最上方。

將標籤 Web 控制項從 [工具箱] 拖曳至設計工具中頁面的頂端。 設定`ID`屬性`StatusLabel`，清除出`Text`屬性，以及組`Visible`和`EnableViewState`屬性`False`。 如我們所見先前的教學課程中，設定`EnableViewState`屬性`False`可讓您以程式設計方式變更標籤的屬性值，並將它們自動還原回其預設值，在後續回傳。 這樣可簡化顯示狀態訊息會消失在後續回傳某些使用者動作的回應中的程式碼。 最後，設定`StatusLabel`控制項 s`CssClass`警告，這是 CSS 類別名稱的屬性中定義`Styles.css`會以大型、 粗體、 斜體紅色字型顯示的文字。

圖 11 顯示 Visual Studio 設計工具之後加入並設定標籤。


[![將 statuslabel 設控制項在兩個面板控制項上方放置](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**圖 11**： 位置`StatusLabel`控制上述兩個面板控制項 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>步驟 3： 顯示之間切換，並插入介面

此時，我們已經針對我們的顯示和插入介面，但我們 re 仍保有兩項工作完成標記：

- 顯示之間切換，並插入介面
- 加入在資料庫的出貨的產品

目前，顯示介面會顯示，但是插入介面隱藏的。 這是因為`DisplayInterface`面板 s`Visible`屬性設定為`True`（預設值），而`InsertingInterface`面板 s`Visible`屬性設定為`False`。 我們只需要切換每個控制項 s 將兩個介面之間切換`Visible`屬性值。

我們想要按一下處理程序的產品出貨按鈕時，顯示介面中移至插入介面。 因此，建立事件處理常式，此按鈕 s`Click`事件包含下列程式碼：


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

此程式碼只會隱藏`DisplayInterface`面板和顯示`InsertingInterface`面板。

接下來，建立事件處理常式將產品從插入介面中的出貨和取消按鈕控制項。 按一下其中一個按鈕時，我們需要還原回顯示介面。 建立`Click`兩個事件處理常式按鈕控制項，好讓它們呼叫`ReturnToDisplayInterface`，我們將會立刻顯示加入的方法。 除了隱藏`InsertingInterface`面板和顯示`DisplayInterface`面板中，`ReturnToDisplayInterface`方法需要 Web 控制項傳回至預先編輯的狀態。 這項作業包括設定 DropDownLists`SelectedIndex`屬性為 0，並清除`Text`文字方塊控制項的內容。

> [!NOTE]
> 請考慮可能會發生什麼事如果我們 professionals t 恢復成預先編輯狀態控制項，再傳回給顯示介面。 使用者可能會按一下處理程序的產品出貨按鈕、 輸入的產品出貨，，然後按一下 新增出貨的產品。 這會將產品，並返回顯示介面中的使用者。 此時使用者可能會想要新增另一個出貨。 按一下 [處理程序的產品出貨] 按鈕就會傳回插入的介面，但 DropDownList 後選取項目和文字方塊值會仍填入其先前的值。


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

同時`Click`只需呼叫事件處理常式`ReturnToDisplayInterface`方法中，雖然我們將傳回要新增的產品出貨從`Click`步驟 4 中的事件處理常式，並將程式碼加入儲存的產品。 `ReturnToDisplayInterface`藉由傳回啟動`Suppliers`和`Categories`DropDownLists 其第一個選項。 兩個常數`firstControlID`和`lastControlID`標記的起始和結束控制項用來命名的產品名稱和單位價格中插入文字方塊介面，並使用的界限內的索引值`For`設定的迴圈`Text`文字方塊控制項的內容傳回空字串。 最後，面板`Visible`屬性重設，以便插入介面隱藏和顯示介面所示。

請花一點時間，若要測試此頁面，在瀏覽器中。 當第一次瀏覽頁面時您應該會看到顯示介面已在 「 圖 5 所示。 按一下處理程序的產品出貨按鈕。 頁面會回傳，您現在應該會看到插入介面圖 12 中所示。 按一下任一個加入的產品出貨 或 取消 5d; 按鈕會傳回顯示介面。

> [!NOTE]
> 在檢視的插入介面時，請花一點時間單位價格的文字方塊上 CompareValidators 測試。 您應該會看到用戶端 messagebox 時按一下 新增產品從無效的貨幣值的出貨按鈕或價格小於零的值與警告。


[![按一下處理程序的產品出貨按鈕後顯示插入介面](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**圖 12**: 插入介面會顯示，按一下處理程序的產品出貨按鈕後 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>步驟 4： 加入產品

著作權所有，本教學課程是將儲存到資料庫中新增的產品的產品，從 「 出貨 」 按鈕的`Click`事件處理常式。 這可藉由建立`ProductsDataTable`並加入`ProductsRow`提供的產品名稱的每個執行個體。 一次這些`ProductsRow`已加入將會進行呼叫`ProductsBLL`類別 s`UpdateWithTransaction`方法傳入`ProductsDataTable`。 請記得，`UpdateWithTransaction`方法上, 一步中所建立[包裝在交易內的資料庫修改](wrapping-database-modifications-within-a-transaction-vb.md)教學課程，傳遞`ProductsDataTable`至`ProductsTableAdapter`s`UpdateWithTransaction`方法。 從該處，ADO.NET 會啟動交易和 TableAdatper 問題`INSERT`陳述式之資料庫的每個已加入`ProductsRow`DataTable 中。 假設所有產品會都加入不會發生錯誤，就會認可交易，否則它就會回復。

新增產品出貨按鈕 s 中的程式碼`Click`事件處理常式也需要執行一些錯誤檢查。 因為沒有 RequiredFieldValidators 用於插入介面，使用者無法輸入價格產品時省略它的名稱。 因為 s 產品的名稱是必要項目，如果這種情況可以掀起我們需要提醒使用者並不會進行插入。 完整`Click`事件處理常式程式碼遵循：


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

如此可確保此事件處理常式會啟動`Page.IsValid`屬性會傳回值為`True`。 如果它傳回`False`、 然後這表示一個或多個 CompareValidators 報告無效的資料; 在此情況下我們不想嘗試插入輸入的產品或我們最後例外狀況時嘗試指派的使用者輸入的單價值設定為`ProductsRow`s`UnitPrice`屬性。

下一步]、 [新`ProductsDataTable`建立執行個體 (`products`)。 A`For`迴圈用來逐一查看的產品名稱和單位價格的文字方塊和`Text`內容讀入區域變數`productName`和`unitPrice`。 如果使用者已輸入的值，單價但沒有對應的產品名稱，`StatusLabel`顯示訊息，如果您提供一個單位價格，您也必須包含的產品名稱和結束事件處理常式。

如果已提供的產品名稱，新`ProductsRow`建立執行個體`ProductsDataTable`s`NewProductsRow`方法。 這個新`ProductsRow`執行個體 s`ProductName`屬性設定為目前的產品名稱文字方塊中時`SupplierID`和`CategoryID`屬性指派給`SelectedValue`DropDownLists 插入介面 s 標頭中的屬性。 如果使用者輸入的產品的價格的值，將它指派給`ProductsRow`執行個體 s`UnitPrice`屬性; 否則屬性是左未指定，會導致`NULL`值`UnitPrice`資料庫中。 最後，`Discontinued`和`UnitsOnOrder`為硬式編碼的值指派給屬性`False`和 0 分別。

屬性已指派給之後`ProductsRow`執行個體加入至`ProductsDataTable`。

在完成`For`迴圈中，我們會檢查是否已新增的任何產品。 使用者可能在所有情況下，按一下新增進入的任何產品名稱或價格之前的出貨的產品。 如果沒有至少一個產品中的`ProductsDataTable`、`ProductsBLL`類別的`UpdateWithTransaction`方法呼叫。 接下來，資料會重新繫結至`ProductsGrid`GridView，使新加入的產品會出現在顯示介面。 `StatusLabel`會更新以顯示確認訊息和`ReturnToDisplayInterface`叫用時，隱藏插入介面和顯示顯示介面。

如果不輸入任何產品，插入的介面，仍會顯示但沒有產品已新增的訊息。 請輸入產品名稱和單價的文字方塊中會顯示。

圖 13、 14 和 15 顯示插入和顯示作用中的介面。 圖 13 在使用者輸入不含對應的產品名稱的單位價格值。 圖 14 顯示顯示介面之後三個新產品已經成功加入，而圖 15 會顯示兩個新加入的產品在 GridView （第三個是前一頁）。


[![產品名稱是必要時輸入 Unit Price](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**圖 13**: 產品名稱是必要時輸入單價 ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image39.png))


[![已加入三個新 Veggies 供應商 Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**圖 14**： 三個新 Veggies 已經加入的供應商 Mayumi s ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image42.png))


[![新的產品可以在 GridView 的最後一頁](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**圖 15**: 新的產品可以在最後一頁的 GridView ([按一下以檢視完整大小的影像](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> 批次插入此教學課程中使用的邏輯包裝插入的交易範圍內。 若要確認這種情況，故意加入資料庫層級錯誤。 例如，與其指派新`ProductsRow`執行個體 s`CategoryID`屬性中選取的值`Categories`DropDownList，它的值要的指派`i * 5`。 這裡`i`迴圈索引子，而且範圍從 1 到 5 的值。 因此，當加入兩個或多項產品在批次中的插入第一項產品必須是有效`CategoryID`值 (5)，但是後續的產品會造成`CategoryID`值不符合最多`CategoryID`值`Categories`資料表。 結果是，當第一個`INSERT`會成功，後續的會因外部索引鍵條件約束違規。 批次插入是不可部分完成，因為第一個`INSERT`將會回復，傳回其狀態的批次插入程序之前的資料庫開始。


## <a name="summary"></a>總結

這與先前的兩個教學課程中，我們已經建立介面，讓更新、 刪除和插入的資料批次，全部都是使用我們加入到資料存取層中的交易支援[包裝資料庫修改在交易內](wrapping-database-modifications-within-a-transaction-vb.md)教學課程。 某些案例中，這類批次處理使用者介面大幅改善使用者的效率剪下向下數目按、 回傳和鍵盤-滑鼠內容切換，同時也會維護基礎資料的完整性。

本教學課程完成我們查看使用批次的資料。 教學課程的下一組探索各種不同的進階資料存取層案例中，包括如何使用預存程序中設定連接和命令層級設定 DAL、 加密的連接字串和更多的 TableAdapter 的方法 ！

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 導致檢閱者在此教學課程已 Hilton Giesenow 和 S ren 因此 Lauritsen。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一步](batch-deleting-vb.md)
