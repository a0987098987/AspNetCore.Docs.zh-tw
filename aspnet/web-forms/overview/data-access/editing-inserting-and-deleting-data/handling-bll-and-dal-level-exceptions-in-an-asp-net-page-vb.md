---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: 處理 BLL 和 DAL 層級 ASP.NET 頁面 (VB) 中的例外狀況 |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將了解如何顯示易懂、 資訊性的錯誤訊息應該在插入、 更新或刪除作業期間發生例外狀況...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: de181391a074ec837d2f9d98d55f912883d76be2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808988"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe)或[下載 PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> 在本教學課程中，我們將了解如何顯示易懂、 資訊性的錯誤訊息應該在插入、 更新或在 ASP.NET 資料 Web 控制項的刪除作業期間發生例外狀況。


## <a name="introduction"></a>簡介

有關使用階層式應用程式架構的 ASP.NET web 應用程式的資料包含下列三個一般步驟：

1. 決定所需要叫用商務邏輯層的何種方法，以及哪些參數值傳遞給它。 參數值可以是硬式編碼，以程式設計的方式指派，或使用者所輸入的輸入。
2. 叫用方法。
3. 處理結果。 呼叫傳回資料的 BLL 方法時，這可能牽涉到的資料繫結至資料 Web 控制項。 如需修改資料的 BLL 方法，這可能包括執行某些動作，根據傳回的值，或依正常程序處理發生在步驟 2 中的任何例外狀況。

如我們在中所見[先前的教學課程](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)，ObjectDataSource 和資料 Web 控制項提供擴充點的步驟 1 和 3。 GridView，比方說，就會引發其`RowUpdating`事件之前將其欄位的值指派給其 ObjectDataSource`UpdateParameters`集合，其`RowUpdated`ObjectDataSource 完成作業後，會引發事件。

我們已經討論過程在步驟 1 期間引發，看過如何使用這些自訂的輸入的參數，或取消作業的事件。 在本教學課程中我們將把焦點轉到作業完成之後引發的事件。 我們也可以在其他方面，這些層級後置的事件處理常式判斷作業期間是否發生例外狀況，並依正常程序，處理它，在螢幕上顯示易懂、 資訊性的錯誤訊息，而不是預設為標準的 ASP.NET例外狀況 頁面中。

為了說明使用這些後置的層級的事件，讓我們建立頁面，其中列出可編輯的 GridView 內的產品。 更新產品，如果有例外狀況引發時我們的 ASP.NET 頁面會顯示上面 GridView 說明已發生問題的簡短訊息。 讓我們開始吧 ！

## <a name="step-1-creating-an-editable-gridview-of-products"></a>步驟 1： 建立產品的可編輯的 GridView

先前的教學課程中我們建立兩個欄位，可編輯的 GridView`ProductName`和`UnitPrice`。 這需要建立的其他多載`ProductsBLL`類別的`UpdateProduct`方法，其中每個產品欄位相的參數只接受三個輸入的參數 （產品名稱、 單價和 ID）。 本教學課程中，讓我們來練習這項技術同樣地，建立可編輯的 GridView 庫存，顯示產品的名稱，每個單元測試、 單價和單位的數量，但只允許在編輯庫存中的 名稱、 單價和單位。

為滿足此案例中，我們再需要另一個多載`UpdateProduct`方法，它會接受四個參數： 產品名稱、 單價、 單位股票圖、 和識別碼。 將下列方法加入`ProductsBLL`類別：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

使用此完整的方法，我們已準備好建立 ASP.NET 網頁，以便編輯這四個特定產品的欄位。 開啟`ErrorHandling.aspx`頁面中`EditInsertDelete`資料夾，並將透過設計工具頁面上的 GridView。 繫結至新的 ObjectDataSource 的 GridView 對應`Select()`方法，以`ProductsBLL`類別的`GetProducts()`方法和`Update()`方法，以`UpdateProduct`剛才建立的多載。


[![使用 UpdateProduct 方法多載可接受四個輸入的參數](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**圖 1**： 使用`UpdateProduct`方法多載，接受四個輸入參數 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


這會建立與 ObjectDataSource`UpdateParameters`有四個參數和欄位使用 GridView 的每個產品欄位的集合。 ObjectDataSource 的宣告式標記指派`OldValuesParameterFormatString`屬性值`original_{0}`，這會導致例外狀況，因為我們的 BLL 類別不會預期輸入的參數名稱為`original_productID`必須傳入。 別忘了移除這項設定完全宣告式語法從 (或將它設定為預設值， `{0}`)。

接下來，削減為只包含 GridView `ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`BoundFields。 也請自由套用您覺得有必要的任何欄位層級格式 (例如變更`HeaderText`屬性)。

在上一個教學課程中我們探討了如何格式化`UnitPrice`BoundField 在唯讀模式和編輯模式中的貨幣。 讓我們這裡的相同。 還記得，這需要設定 BoundField`DataFormatString`屬性，以`{0:c}`、 其`HtmlEncode`屬性設`false`，並將其`ApplyFormatInEditMode`至`true`，如 [圖 2] 所示。


[![設定 UnitPrice BoundField，顯示為貨幣](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**圖 2**： 設定`UnitPrice`BoundField 顯示為貨幣 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


格式化`UnitPrice`編輯介面中的貨幣需要建立事件處理常式，如 GridView`RowUpdating`貨幣格式化字串剖析成的事件`decimal`值。 請記得，`RowUpdating`也檢查中的最後一個教學課程中的事件處理常式，以確保使用者提供`UnitPrice`值。 不過，本教學課程中我們允許使用者略過價格。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

我們的 GridView 包含`QuantityPerUnit`BoundField，但此 BoundField 應僅供顯示之用，且不應由使用者可編輯。 若要排列這種情況，只要設定 BoundFields'`ReadOnly`屬性設`true`。


[![將 QuantityPerUnit BoundField 唯讀](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**圖 3**： 請`QuantityPerUnit`BoundField 唯讀狀態 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


最後，核取方塊啟用編輯從 GridView 的智慧標籤。 完成這些步驟之後`ErrorHandling.aspx`網頁的設計工具應該看起來會類似 圖 4。


[![全部移除需要 BoundFields 和核取 [啟用編輯] 核取方塊](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**圖 4**： 移除以外的所有所需 BoundFields 和核取 啟用編輯核取方塊 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


現在我們有一份所有產品的`ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`欄位; 不過，只有`ProductName`， `UnitPrice`，和`UnitsInStock`可編輯欄位。


[![使用者現在可以輕鬆地編輯產品的名稱、 價格和內建欄位中的單位](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**圖 5**： 使用者可以立即輕鬆編輯產品的名稱、 價格和單位中內建欄位 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>步驟 2： 依正常程序處理 DAL 層級的例外狀況

雖然我們可以讓您編輯的 GridView 作出可行時使用者輸入的已編輯的產品名稱、 價格和庫存單位數的法律的值，輸入不合法的值會導致例外狀況。 例如，省略`ProductName`值的原因[NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp)自擲回`ProductName`中的屬性`ProdcutsRow`類別具有其`AllowDBNull`屬性設定為`false`; 如果資料庫已關閉，`SqlException`將會擲回 TableAdapter 時嘗試連線到資料庫。 但不採取任何動作，這些例外狀況反昇資料存取層的商業邏輯層，然後 ASP.NET 頁面中，最後到 ASP.NET 執行階段。

根據您的 web 應用程式的設定方式和是否您造訪應用程式從`localhost`，未處理的例外狀況可能會導致一般伺服器錯誤頁面、 詳細的錯誤報表或使用者易記的網頁。 請參閱[Web 應用程式在 ASP.NET 中處理的錯誤](http://www.15seconds.com/issue/030102.htm)並[customErrors 項目](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx)如需有關 ASP.NET 執行階段回應無法攔截的例外狀況的方式。

圖 6 顯示嘗試更新產品，但未指定時所遇到的畫面`ProductName`值。 這是的預設的通過時顯示詳細的錯誤報告`localhost`。


[![省略產品的名稱會顯示例外狀況詳細資料](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**圖 6**： 省略產品的名稱會顯示例外狀況詳細資料 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


雖然這類例外狀況詳細資料很有用，測試應用程式時，向使用者顯示例外狀況時的這類的畫面是不盡理想。 使用者可能不知道該`NoNullAllowedException`是或它所造成的原因。 更好的方法是顯示給使用者的使用者更容易使用的訊息，說明已嘗試將產品更新的問題。

如果執行作業時，就會發生例外狀況，ObjectDataSource 和資料 Web 控制項中的後續層級的事件會提供方法來偵測到它，並取消從事件反昇至 ASP.NET 執行階段例外狀況。 我們的範例，讓我們建立事件處理常式 」 的 GridView 的`RowUpdated`判斷例外狀況已引發，若是如此，則顯示例外狀況詳細資料標籤 Web 控制項中的事件。

開始將標籤新增至 ASP.NET 頁面上，設定其`ID`屬性，以`ExceptionDetails`並清除其`Text`屬性。 若要繪製使用者的眼睛，此訊息，請設定其`CssClass`屬性，以`Warning`，這是我們所新增的 CSS 類別`Styles.css`先前的教學課程中的檔案。 您應該記得，這個 CSS 類別會造成標籤的文字，以顯示紅色、 斜體、 粗體、 超大的字型。


[![將標籤的 Web 控制項加入頁面](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**圖 7**： 將標籤 Web 控制項新增至網頁 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


因為我們希望此標籤的 Web 控制項成為可見只立即之後發生的例外狀況，請將設定其`Visible`屬性設定為 false，在`Page_Load`事件處理常式：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

此程式碼，在第一個頁面瀏覽和後續的回傳`ExceptionDetails`控制項會有其`Visible`屬性設定為`false`。 面對的 DAL 層級或 BLL 層級例外狀況，我們可偵測的 GridView`RowUpdated`事件處理常式中，我們會設定`ExceptionDetails`控制項的`Visible`屬性設為 true。 因為 Web 控制項事件處理常式之後發生`Page_Load`頁面生命週期中的事件處理常式，將會顯示標籤。 不過，在下一個回傳時，`Page_Load`事件處理常式將會還原`Visible`屬性回`false`，再從檢視隱藏它。

> [!NOTE]
> 另外，我們可以移除設定的必要性`ExceptionDetails`控制項的`Visible`中的屬性`Page_Load`藉由指派其`Visible`屬性`false`中的宣告式語法，並停用 （設定其其檢視狀態`EnableViewState`屬性設`false`)。 我們將在未來的教學課程中使用此替代方法。


新增標籤控制項下, 一步是建立事件處理常式的 GridView 的`RowUpdated`事件。 選取 GridView 設計工具中，移至 [屬性] 視窗中，然後按一下閃電圖示，列出 GridView 的事件。 應該已有的項目有 GridView 的`RowUpdating`事件，因為我們稍早在本教學課程中建立此事件的事件處理常式。 建立事件處理常式`RowUpdated`以及事件。


![GridView 的 RowUpdated 事件建立事件處理常式](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**圖 8**： 建立事件處理常式，如 GridView 的`RowUpdated`事件


> [!NOTE]
> 您也可以建立事件處理常式，以透過下拉式清單頂端的程式碼後置類別檔案。 從左側下拉式清單中選取 GridView 和`RowUpdated`從右側的一個事件。


建立這個事件處理常式會將下列程式碼新增至 ASP.NET 網頁的程式碼後置類別：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

這個事件處理常式的第二個輸入的參數是型別的物件[GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)，其中包含處理例外狀況的感興趣的三個屬性：

- `Exception` 擲回的例外狀況; 參考如果已擲不回任何例外狀況，此屬性會有值 `null`
- `ExceptionHandled` 布林值，指出是否在已處理的例外狀況`RowUpdated`事件處理常式; 如果`false`（預設）、 例外狀況會重新擲回，percolating 至 ASP.NET 執行階段
- `KeepInEditMode` 如果設定為`true`編輯的 GridView 資料列處於編輯模式; 如果會維持`false`（預設值），GridView 資料列會還原回其唯讀模式

然後，我們的程式碼，應該檢查以查看是否`Exception`不是`null`，這表示執行作業時引發的例外狀況。 如果發生這種情況，我們想要：

- 顯示簡單易懂的訊息中`ExceptionDetails`標籤
- 表示已處理例外狀況
- 保留的 GridView 資料列處於編輯模式

下列程式碼會完成這些目標：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

這個事件處理常式一開始先查看是否`e.Exception`是`null`。 如果未列出，請`ExceptionDetails`標籤的`Visible`屬性設定為`true`及其`Text`屬性，以 「 發生問題，更新產品。 」 實際擲回的例外狀況的詳細資料位於`e.Exception`物件的`InnerException`屬性。 會檢查這個內部例外狀況，且其他，很有幫助的訊息是否為特定類型的範例，附加至`ExceptionDetails`標籤的`Text`屬性。 最後，`ExceptionHandled`並`KeepInEditMode`屬性都設為`true`。

圖 9] 顯示此頁面的螢幕擷取畫面時省略產品; 名稱[圖 10 顯示時輸入不正確的結果`UnitPrice`值 (-50)。


[![ProductName BoundField 必須包含值。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**圖 9**: `ProductName` BoundField 必須包含值 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![負值 UnitPrice 是不允許](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**圖 10**： 負數`UnitPrice`的值為不允許 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


藉由設定`e.ExceptionHandled`屬性，以`true`，則`RowUpdated`事件處理常式已指出它已處理的例外狀況。 因此，例外狀況不會傳播至 ASP.NET 執行階段。

> [!NOTE]
> 圖 9 和 10 顯示優雅的方式，來處理由於無效的使用者輸入所產生的例外狀況。 在理想情況下，不過，這類輸入無效，將不會觸達商務邏輯層一開始，因為 ASP.NET 網頁應該確保使用者的輸入都有效，然後再叫用`ProductsBLL`類別的`UpdateProduct`方法。 在我們下一個教學課程中我們將了解如何將驗證控制項新增至編輯和插入介面，以確保送出至商業邏輯層的資料符合商務規則。 驗證控制項不僅會防止的引動過程`UpdateProduct`方法，直到使用者提供的資料有效，但也提供用來識別資料輸入問題的更具參考性的使用者體驗。


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>步驟 3︰ 依正常程序處理 BLL 層級的例外狀況

插入時，更新或刪除資料，資料存取層可能會擲回例外狀況時的資料相關的錯誤。 資料庫可能已離線、 必要的資料庫資料表資料行可能不具有指定值或資料表層級條件約束違規。 嚴格的資料相關的例外狀況，除了商務邏輯層可以使用例外狀況，表示已違反商務規則時。 在 [建立商業邏輯層](../introduction/creating-a-business-logic-layer-vb.md)教學課程中，比方說，我們加入商務規則檢查原始`UpdateProduct`多載。 具體來說，如果使用者已標示的產品，為已停止，我們需要產品不是唯一的產品的供應商所提供。 如果已違反此條件，`ApplicationException`擲回。

針對`UpdateProduct`多載建立本教學課程中，將它加入 商務規則，以禁止`UnitPrice`欄位設定為兩倍以上的原始和新值從`UnitPrice`值。 若要達成此目的，調整`UpdateProduct`多載，讓它執行這項檢查，並擲回`ApplicationException`如果違反規則。 已更新的方法如下所示：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

這項變更，是兩倍以上的現有價格的任何價格更新會導致`ApplicationException`擲回。 就像從 DAL，此 BLL 引發所引發的例外狀況`ApplicationException`可以偵測並處理在 GridView 的`RowUpdated`事件處理常式。 事實上，`RowUpdated`事件處理常式的程式碼，如所撰寫，會正確偵測此例外狀況並顯示`ApplicationException`的`Message`屬性值。 [圖 11] 顯示螢幕擷取畫面，當使用者嘗試更新 $50.00，這是多個 double 其股票的現價 $19.95 Chai 的價格。


[![商務規則不允許超過兩倍的產品價格的價格會增加](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**圖 11**: 商務規則不允許超過兩倍的產品價格漲價 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> 在理想情況下我們的商務邏輯規則應重整共`UpdateProduct`方法多載和常見的方法。 這則留待練習，讀取器。


## <a name="summary"></a>總結

在插入、 更新和刪除作業，同時資料 Web 控制項和所涉及的 ObjectDataSource 引發前置和後置的層級事件該書夾的實際作業。 如我們所見在本教學課程和上例中，當使用可編輯的 GridView GridView`RowUpdating`事件引發時，後面接著 ObjectDataSource 的`Updating`事件，此時更新命令對 ObjectDataSource 的基礎物件。 作業完成後，ObjectDataSource 後`Updated`事件引發時，後面接著 GridView 的`RowUpdated`事件。

我們可以建立事件處理常式中，使用預先層級的事件，若要自訂的輸入的參數或後置的層級的事件，以檢查及回應作業的結果。 後置的層級的事件處理常式最常用來偵測作業期間是否發生例外狀況。 發生例外狀況，這些層級後置的事件處理常式可選擇性地處理自己的例外狀況。 在本教學課程中，我們了解如何處理這類例外狀況顯示易懂的錯誤訊息的內容。

在下一個教學課程中，我們將看到如何減少從資料格式設定問題所造成的例外狀況的可能性 (例如輸入負`UnitPrice`)。 具體來說，我們將探討如何將驗證控制項新增至編輯和插入介面。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [下一頁](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
