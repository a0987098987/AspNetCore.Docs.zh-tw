---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: BLL 和 DAL 層級中處理例外狀況是 ASP.NET 網頁 (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們會看到如何顯示好記且有意義的錯誤訊息應該插入、 更新或刪除作業期間發生例外狀況...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b76554b6e8c00dbe3b33de8158b925d7314afb72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>ASP.NET 網頁 (VB) 中的 BLL 和 DAL 層級例外狀況處理
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe)或[下載 PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> 在本教學課程中，我們會看到如何顯示好記且有意義的錯誤訊息應該插入、 更新或刪除作業的 Web 控制項的 ASP.NET 資料期間發生例外狀況。


## <a name="introduction"></a>簡介

使用 ASP.NET web 應用程式使用多層應用程式架構的資料包含下列三個一般步驟：

1. 決定哪一個方法的商務邏輯層需要叫用，以及哪些參數值，將它傳遞。 參數值可以是硬式編碼，以程式設計方式指定，或使用者所輸入的輸入。
2. 叫用方法。
3. 處理結果。 呼叫傳回資料 BLL 方法時，這可能會將資料繫結至資料 Web 控制項。 對於 BLL 修改資料的方法，這可能包括執行某些動作傳回的值為基礎，或者依正常程序處理會在步驟 2 中發生任何例外狀況。

如我們所見中[上一個教學課程](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)，ObjectDataSource 和 Web 控制項的資料會提供擴充性點步驟 1 和 3。 例如，就會引發 GridView 中其`RowUpdating`事件之前將其欄位的值指派給其 ObjectDataSource`UpdateParameters`集合; 其`RowUpdated`ObjectDataSource 完成作業之後，就會引發事件。

我們已經已檢查過的事件，在步驟 1 期間引發，並已經看到如何使用它們自訂的輸入的參數，或取消作業。 在此教學課程中我們將會開啟我們注意到在作業完成之後引發的事件。 我們也可以在其他方面，這些後置層級的事件處理常式以判斷作業期間是否發生例外狀況，並處理依正常程序，在螢幕上顯示的好記且有意義的錯誤訊息，而不是預設為標準 ASP.NET例外狀況 頁面中。

為了說明使用這些後置層級的事件，讓我們來建立頁面，其中列出可編輯的 GridView 中的產品。 更新產品，如果例外狀況時引發我們 ASP.NET 頁面會顯示上面 GridView 說明發生問題的簡短訊息。 讓我們開始吧 ！

## <a name="step-1-creating-an-editable-gridview-of-products"></a>步驟 1： 建立產品的可編輯的 GridView

在上一個教學課程中我們建立兩個欄位，可編輯的 GridView`ProductName`和`UnitPrice`。 這需要建立的其他多載`ProductsBLL`類別的`UpdateProduct`方法，一個用於每個產品欄位相對於的參數只接受三個輸入的參數 （產品名稱、 單價及識別碼）。 本教學課程，讓我們來練習這項技術同樣地，建立可編輯的 GridView 會顯示股票圖、 產品的名稱，每個單元測試、 單價和單位的數量，但只允許的名稱、 單價和單位，可供編輯的庫存。

為配合這種情況下，我們再需要另一個多載的`UpdateProduct`方法中，接受四個參數： 產品的名稱、 單價、 單位在股票圖、 和識別碼。 將下列方法加入`ProductsBLL`類別：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

完成此方法中，我們就可以建立 ASP.NET 頁面，允許編輯這四個特定產品欄位。 開啟`ErrorHandling.aspx`頁面`EditInsertDelete`資料夾並加入透過設計工具頁面的 GridView。 將 GridView 繫結至新 ObjectDataSource，對應`Select()`方法`ProductsBLL`類別的`GetProducts()`方法和`Update()`方法`UpdateProduct`剛才建立的多載。


[![使用接受四個輸入的參數的 UpdateProduct 方法多載](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**圖 1**： 使用`UpdateProduct`方法多載，接受四個輸入參數 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


這會建立與 ObjectDataSource`UpdateParameters`有四個參數和欄位的 GridView 每個產品欄位的集合。 ObjectDataSource 的宣告式標記指派`OldValuesParameterFormatString`屬性值`original_{0}`，這將會導致例外狀況，因為我們的 BLL 類別不需要輸入的參數名稱為`original_productID`傳入。 別忘了移除這項設定完全宣告式語法 (或將它設定為預設值， `{0}`)。

接下來，為只包含 GridView 先行削減`ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`BoundFields。 您也可隨時套用您覺得有必要的任何欄位層級格式 (例如變更`HeaderText`屬性)。

在上一個教學課程中我們探討如何格式化`UnitPrice`BoundField 做為貨幣符號在唯讀模式和編輯模式。 就來看看相同這裡。 前文提過，這需要設定 BoundField`DataFormatString`屬性`{0:c}`、 其`HtmlEncode`屬性`false`，及其`ApplyFormatInEditMode`至`true`，如圖 2 所示。


[![設定 UnitPrice BoundField，顯示為貨幣](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**圖 2**： 設定`UnitPrice`BoundField 顯示為貨幣 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


格式化`UnitPrice`貨幣編輯介面中的需要建立事件處理常式的 GridView`RowUpdating`事件的貨幣格式字串剖析成`decimal`值。 請記得，`RowUpdating`事件處理常式，從上一個教學課程也會檢查以確保使用者提供`UnitPrice`值。 不過，本教學課程中我們讓使用者省略價格。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

我們 GridView 包含`QuantityPerUnit`BoundField，但此 BoundField 應該是只適用於顯示用途，而且不應該是可編輯的使用者。 若要排列這種情況，只要設定 BoundFields'`ReadOnly`屬性`true`。


[![將 QuantityPerUnit BoundField 唯讀](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**圖 3**： 請`QuantityPerUnit`BoundField 唯讀 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


最後，請檢查 GridView 的智慧標籤從 啟用編輯核取方塊。 完成這些步驟之後`ErrorHandling.aspx`頁面的設計工具看起來應該類似於圖 4。


[![全部移除需要 BoundFields 和核取 [啟用編輯] 核取方塊](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**圖 4**： 移除所有需要 BoundFields 和核取 啟用編輯核取方塊 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


現在我們有一份所有產品的`ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`欄位; 不過，只有`ProductName`， `UnitPrice`，和`UnitsInStock`可編輯欄位。


[![使用者現在可以輕鬆地編輯產品的名稱、 價格以及內建欄位中的單位](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**圖 5**： 使用者可以立即輕鬆地編輯產品的名稱、 價格以及單位中內建欄位 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>步驟 2： 依正常程序 DAL 層級例外狀況處理

當我們可以讓您編輯 GridView 作出當使用者輸入的已編輯的產品名稱、 價格和庫存單位的合法值，輸入不合法的值會導致例外狀況。 例如，省略`ProductName`值原因[NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp)之後擲回`ProductName`中的屬性`ProdcutsRow`類別具有其`AllowDBNull`屬性設定為`false`; 如果資料庫已關閉，`SqlException`會擲回的 TableAdapter 時嘗試連線到資料庫。 但不採取任何動作，這些例外狀況反昇資料存取層的商務邏輯層，然後再 ASP.NET 頁面中，最後再 ASP.NET 執行階段。

根據您的 web 應用程式的設定方式和是否您瀏覽應用程式從`localhost`，未處理的例外狀況可能會導致一般伺服器錯誤頁面、 詳細的錯誤報表或使用者易記的網頁。 請參閱[Web 應用程式 Error Handling in ASP.NET](http://www.15seconds.com/issue/030102.htm)和[customErrors 元素](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx)如需有關 ASP.NET 執行階段如何回應無法攔截的例外狀況。

圖 6 顯示螢幕時嘗試更新的產品，但未指定，發現`ProductName`值。 這是來自時顯示詳細的錯誤報告的預設`localhost`。


[![省略產品的名稱會顯示例外狀況詳細資料](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**圖 6**： 省略產品的名稱會顯示例外狀況詳細資料 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


雖然測試應用程式時，這類例外狀況詳細資料很有用，這類螢幕遇到例外狀況時呈現給終端使用者是不盡理想。 使用者可能無法辨識`NoNullAllowedException`是或它所造成的原因。 更好的方法是對使用者顯示更加易懂易記的訊息，說明已嘗試將產品更新的問題。

如果執行作業時，就會發生例外狀況，ObjectDataSource 和資料的 Web 控制項中的後置層級事件會提供方法來偵測和取消的例外狀況反昇至 ASP.NET 執行階段。 我們的範例，讓我們來建立事件處理常式 」 的 GridView`RowUpdated`判斷例外狀況已引發，若是如此，則顯示例外狀況詳細資料標籤 Web 控制項中的事件。

將標籤加入至 ASP.NET 頁面上，設定啟動其`ID`屬性`ExceptionDetails`和清除其`Text`屬性。 若要繪製使用者的眼睛，這個訊息，請設定其`CssClass`屬性`Warning`，我們加入的 CSS 類別`Styles.css`在上一個教學課程中的檔案。 前文提過這個 CSS 類別，會顯示紅色、 粗體、 斜體超大型字型的標籤文字。


[![將標籤 Web 控制項加入頁面](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**圖 7**： 將標籤 Web 控制項加入頁面 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


因為我們想要這個標籤 Web 控制項才能看到只有立即之後發生了例外狀況，請將設定其`Visible`屬性設定為 false，在`Page_Load`事件處理常式：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

使用此程式碼，在第一個頁面瀏覽和後續回傳時`ExceptionDetails`控制項會有其`Visible`屬性設定為`false`。 DAL 或 BLL 層級例外狀況，我們可以偵測到在 GridView 中面臨`RowUpdated`事件處理常式中，我們會設定`ExceptionDetails`控制項的`Visible`屬性設定為 true。 由於 Web 控制項的事件處理常式發生之後`Page_Load`頁面生命週期中的事件處理常式，將會顯示標籤。 不過，在下一步的回傳，`Page_Load`事件處理常式將會還原`Visible`屬性設回`false`，再次隱藏檢視。

> [!NOTE]
> 另外，我們可以移除您不再需要設定`ExceptionDetails`控制項的`Visible`屬性`Page_Load`藉由指派其`Visible`屬性`false`宣告式語法和停用 （設定其其檢視狀態中`EnableViewState`屬性`false`)。 我們將在未來的教學課程中使用這個替代方法。


加入 Label 控制項下, 一步是建立事件處理常式的 GridView`RowUpdated`事件。 GridView 設計工具中選取，請移至 [屬性] 視窗中，然後按一下出現閃電圖示，列出 GridView 的事件。 應該已經有的項目有 GridView`RowUpdating`事件，我們稍早在本教學課程中建立此事件的事件處理常式。 建立事件處理常式`RowUpdated`以及事件。


![在 GridView 的 RowUpdated 事件建立事件處理常式](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**圖 8**： 建立事件處理常式的 GridView`RowUpdated`事件


> [!NOTE]
> 您也可以建立事件處理常式，透過下拉式清單上方的程式碼後置類別檔。 從左側下拉式清單中選取 GridView 和`RowUpdated`右側上的事件。


建立此事件處理常式會將下列程式碼新增到 ASP.NET 網頁的程式碼後置類別：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

此事件處理常式的第二個輸入的參數是類型的物件[GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)，其具有三個處理例外狀況的感興趣的屬性：

- `Exception` 擲回的例外狀況; 的參考如果已擲不回任何例外狀況，此屬性會有的值 `null`
- `ExceptionHandled` 布林值，指出是否已處理的例外狀況中`RowUpdated`事件處理常式; 如果`false`（預設值），例外狀況會重新擲回，percolating 至 ASP.NET 執行階段
- `KeepInEditMode` 如果設定為`true`編輯的 GridView 資料列處於編輯模式; 如果`false`（預設）、 GridView 資料列會還原回其為唯讀模式

然後，我們的程式碼，應該檢查是否`Exception`不`null`，這表示執行作業時引發的例外狀況。 如果這種情況，我們想要：

- 顯示中的易記訊息`ExceptionDetails`標籤
- 表示已處理的例外狀況
- 保留 GridView 資料列處於編輯模式

下列程式碼會完成下列目標：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

這個事件處理常式開始檢查是否`e.Exception`是`null`。 如果不是，`ExceptionDetails`標籤的`Visible`屬性設定為`true`及其`Text`屬性，以 「 沒有更新的產品問題。 」 實際擲回的例外狀況的詳細資料位於`e.Exception`物件的`InnerException`屬性。 會檢查此內部例外狀況，並額外，很有幫助的訊息是否具有特定型別時，附加至`ExceptionDetails`標籤的`Text`屬性。 最後，`ExceptionHandled`和`KeepInEditMode`屬性都設定為`true`。

圖 9 省略產品; 名稱時，顯示此頁面的螢幕擷取畫面圖 10 顯示時輸入正確的結果`UnitPrice`值 (-50)。


[![ProductName BoundField 必須包含值。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**圖 9**: `ProductName` BoundField 必須包含的值 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![負值 UnitPrice 是不允許](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**圖 10**： 負數`UnitPrice`不允許的值為 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


藉由設定`e.ExceptionHandled`屬性`true`、`RowUpdated`事件處理常式已指出它是否已經處理過例外狀況。 因此，例外狀況不會傳播至 ASP.NET 執行階段。

> [!NOTE]
> 數字 9 和 10 顯示依正常程序來處理由於無效的使用者輸入所產生的例外狀況。 在理想情況下，不過，這類輸入無效，將不會到達商務邏輯層矛盾，為 ASP.NET 網頁應該確定使用者的輸入都有效，再叫用`ProductsBLL`類別的`UpdateProduct`方法。 在我們下一個教學課程中我們會看到如何將驗證控制項的編輯和插入介面來確保資料提交至商務邏輯層加入符合商務規則。 驗證控制項不僅會防止的引動過程`UpdateProduct`方法，直到使用者提供的資料有效，但也提供更具資訊性的使用者經驗來識別資料輸入問題。


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>步驟 3： 依正常程序 BLL 層級例外狀況處理

插入時，更新或刪除資料，資料存取層可能會擲回例外狀況時的資料相關錯誤。 資料庫可能已離線、 必要的資料庫資料表資料行可能未在指定的值或資料表層級條件約束違規。 除了嚴格與資料相關的例外狀況，商務邏輯層可以使用，表示已違反商務規則的例外狀況。 在[建立商務邏輯層](../introduction/creating-a-business-logic-layer-vb.md)教學課程中，例如，我們加入商務規則檢查原始`UpdateProduct`多載。 具體來說，如果使用者已將標示產品，為已停止，我們需要產品不是只有一個產品的供應商所提供。 如果已違反此條件，`ApplicationException`擲回。

如`UpdateProduct`多載建立本教學課程中，讓我們加入商務規則，以禁止`UnitPrice`欄位設定為新值的兩倍以上的原始`UnitPrice`值。 若要完成這項作業，調整`UpdateProduct`多載，以便執行這項檢查，並擲回`ApplicationException`如果違反規則。 已更新的方法如下所示：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

透過這項變更，會導致任何價格所更新的兩倍以上的現有價格`ApplicationException`擲回。 就像從 DAL 這 BLL 引發引發的例外狀況`ApplicationException`可以偵測到並在 GridView 中處理`RowUpdated`事件處理常式。 事實上，`RowUpdated`事件處理常式的程式碼中，當寫入時，會正確地偵測到這個例外狀況並顯示`ApplicationException`的`Message`屬性值。 圖 11 顯示螢幕擷取畫面，當使用者嘗試更新 Chai 價格為 $50.00，這是多個 double $19.95 其目前價格。


[![商務規則不允許多個連產品價格的價格增加](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**圖 11**: 商務規則不允許多個連產品價格的價格增加 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> 我們的商務邏輯規則會在理想情況下重構超出`UpdateProduct`方法多載，並置於常見的方法。 這是保留做為練習，讀取器。


## <a name="summary"></a>總結

在插入、 更新和刪除作業，資料 Web 控制項和涉及的 ObjectDataSource 引發前置和後置層級事件該書夾的實際作業。 因為我們了解在本教學課程和前一個，當使用可編輯的 GridView GridView`RowUpdating`事件引發，後面接著 ObjectDataSource`Updating`事件，此時更新命令對 ObjectDataSource基礎物件。 作業已完成，ObjectDataSource 之後`Updated`事件引發，後面接著 GridView`RowUpdated`事件。

我們可以建立事件處理常式的預先層級的事件，以自訂的輸入的參數或後置層級事件，以檢查及回應作業的結果。 後置層級的事件處理常式最常用來偵測是否在作業期間發生例外狀況。 遇到例外狀況時這些後置層級的事件處理常式可以選擇性地處理自己的例外狀況。 在此教學課程中我們可了解如何處理這類例外狀況顯示易懂的錯誤訊息。

在下一個教學課程中，我們會看到如何降低資料格式設定問題所引起的例外狀況的可能性 (例如輸入負`UnitPrice`)。 具體來說，我們會探討如何加入驗證控制項的編輯和插入的介面。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [下一頁](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
