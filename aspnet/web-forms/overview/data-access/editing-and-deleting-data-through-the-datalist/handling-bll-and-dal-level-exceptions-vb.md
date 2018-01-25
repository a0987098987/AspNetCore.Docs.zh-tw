---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: "處理 BLL 和 DAL 層級例外狀況 (VB) |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們會看到如何巧妙地向處理可編輯的 DataList 更新工作流程期間引發的例外狀況。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bd2ccb13c44d104e8945840705a21738d8abd5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>處理 BLL 和 DAL 層級例外狀況 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe)或[下載 PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> 在本教學課程中，我們會看到如何巧妙地向處理可編輯的 DataList 更新工作流程期間引發的例外狀況。


## <a name="introduction"></a>簡介

在[概觀的編輯和刪除資料在 DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)教學課程中，我們建立 DataList 提供簡單的編輯和刪除功能。 完整的功能，而這是難以方便使用，以編輯期間發生任何錯誤，或刪除處理程序會導致未處理的例外狀況。 例如，省略產品的名稱，或編輯時產品，輸入的價格值非常合理 ！，就會擲回例外狀況。 在程式碼中不會攔截此例外，因為它會顯示最多 ASP.NET 執行階段，然後在網頁中顯示的 s 的例外狀況詳細資料。

如我們所見中[處理 BLL-以及 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程中，如果您的商務邏輯或資料存取層級，從引發例外狀況的例外狀況詳細資料會傳回到 ObjectDataSource 和然後至 GridView。 我們了解如何順暢地處理這些例外狀況，藉由建立`Updated`或`RowUpdated`ObjectDataSource 或 GridView 中檢查例外狀況，並接著指出 已處理此例外狀況的事件處理常式。

我們 DataList 教學課程中，不過，和不 t ObjectDataSource 用於更新和刪除資料。 相反地，我們正在直接針對 BLL。 若要偵測來自 BLL 或 DAL 的例外狀況，我們要實作例外狀況處理程式碼後置的我們的 ASP.NET 頁面內的程式碼。 在本教學課程中，我們會看到如何巧妙地多向處理可以讓您編輯 DataList s 的更新工作流程期間引發的例外狀況。

> [!NOTE]
> 在*概觀的編輯和刪除的資料在 DataList*教學課程中我們將討論不同技術可編輯和刪除資料清單中的資料，來更新使用 ObjectDataSource 涉及一些技術和正在刪除。 如果您使用這些技術時，您可以處理從 BLL 或 DAL ObjectDataSource s 程式的例外狀況`Updated`或`Deleted`事件處理常式。


## <a name="step-1-creating-an-editable-datalist"></a>步驟 1： 建立可編輯的資料清單

我們會擔心更新工作流程期間所發生的例外狀況處理之前，可讓第一次建立可編輯的資料清單。 開啟`ErrorHandling.aspx`頁面`EditDeleteDataList`資料夾中，DataList 加入設計工具中，設定其`ID`屬性`Products`，並新增名為新 ObjectDataSource `ProductsDataSource`。 設定用於 ObjectDataSource`ProductsBLL`類別的`GetProducts()`選取方法記錄; 設定下拉式清單中插入、 更新和刪除索引標籤，以 （無）。


[![傳回的產品資訊。 使用 GetProducts() 方法](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**圖 1**： 傳回產品資訊使用`GetProducts()`方法 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


完成 ObjectDataSource 精靈之後，Visual Studio 會自動建立`ItemTemplate`的資料清單。 取代此`ItemTemplate`，會顯示每個產品的名稱和價格，並包含編輯 按鈕。 接下來，建立`EditItemTemplate`與文字方塊中的 Web 控制項的名稱、 價格和更新 和 取消 5d; 按鈕。 最後，設定 DataList 的`RepeatColumns`2 的屬性。

這些變更之後，請頁面 s 的宣告式標記看起來應該如下所示。 已更新 按鈕，再次以編輯 取消，請確認其`CommandName`編輯、 取消，並更新，請分別設定的屬性。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> 在此教學課程 DataList 必須啟用 s 檢視狀態。


請花一點時間來檢視進度透過瀏覽器 （請參閱圖 2）。


[![每個產品包含編輯 按鈕](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**圖 2**： 每個產品包含 [編輯] 按鈕 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


目前，[編輯] 按鈕只會導致回傳它規定 t 尚未製作產品可編輯。 若要啟用編輯，我們要建立事件處理常式，如 DataList s `EditCommand`， `CancelCommand`，和`UpdateCommand`事件。 `EditCommand`和`CancelCommand`事件只會更新 DataList 的`EditItemIndex`屬性和重新繫結至 DataList 資料：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand`事件處理常式會稍微更為複雜。 它需要讀取已編輯的產品 s`ProductID`從`DataKeys`s 產品名稱以及產品中的文字方塊集合`EditItemTemplate`，然後呼叫`ProductsBLL`類別的`UpdateProduct`方法，再傳回 DataList為其預先編輯的狀態。

現在，讓 s 只使用完全相同的程式碼，從`UpdateCommand`中的事件處理常式*概觀的編輯和刪除資料在 DataList*教學課程。 我們會將新增的程式碼能夠順暢地處理步驟 2 中的例外狀況。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

遇到無效的輸入時，這可能格式不正確的單價、 不合法的單位價格值會類似-$5.00 或略過產品 s 名稱將會引發例外狀況的表單中。 因為`UpdateCommand`事件處理常式不包含任何在此時處理程式碼的例外狀況，例外狀況會反昇至 ASP.NET 執行階段，它顯示給使用者 （請參閱圖 3）。


![當未處理的例外狀況發生時，使用者會看到錯誤頁面](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**圖 3**： 未處理的例外狀況發生時，使用者會看到錯誤頁面


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>步驟 2： 依正常程序中處理例外狀況的 UpdateCommand 事件處理常式

在更新的工作流程期間的例外狀況可能會發生在`UpdateCommand`BLL 或 DAL 事件處理常式。 例如，如果使用者輸入太價格昂貴，`Decimal.Parse`陳述式中的`UpdateCommand`事件處理常式將會擲回`FormatException`例外狀況。 當使用者遺漏產品的名稱或價格有負數值，DAL 將會引發例外狀況。

例外狀況發生時，我們想要顯示在頁面本身內資訊訊息。 新增標籤網頁控制項至頁面`ID`設`ExceptionDetails`。 設定標籤的文字將以紅色顯示，超大，藉由指派粗體和斜體字型其`CssClass`屬性`Warning`中定義的 CSS 類別`Styles.css`檔案。

發生錯誤時，我們只想要一次顯示標籤。 也就是說，在後續回傳時，標籤的警告訊息應該會消失。 這可藉由任一清除標籤 s`Text`屬性或設定其`Visible`屬性`False`中`Page_Load`事件處理常式 (如我們所做的上一步中[處理 BLL-和 ASP DAL 層級例外狀況.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)教學課程) 或停用標籤的檢視狀態的支援。 可讓 s 使用第二種選項。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

當引發例外狀況時，我們將指派到的例外狀況的詳細資料`ExceptionDetails`標籤控制項的`Text`屬性。 因為其檢視狀態為停用，在後續回傳時`Text`屬性 s 以程式設計方式變更將會遺失，再還原至預設的文字 （空字串），從而隱藏的警告訊息。

若要判斷很有幫助的訊息顯示在頁面上，以引發錯誤，我們需要加入`Try ... Catch`封鎖`UpdateCommand`事件處理常式。 `Try`部分包含程式碼，可能會導致例外狀況，而`Catch`區塊包含遇到例外狀況時執行的程式碼。 簽出[例外狀況處理基礎觀念](https://msdn.microsoft.com/library/2w8f0bss.aspx)上一節中的.NET Framework 文件，如需詳細資訊`Try ... Catch`區塊。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

內的程式碼所擲回任何類型的例外狀況是當`Try`區塊，`Catch`的區塊程式碼將會開始執行。 就會擲回的例外狀況類型`DbException`， `NoNullAllowedException`， `ArgumentException`，並依此類推，取決於內容，在第一次導致錯誤。 如果那里問題，在資料庫層級`DbException`就會擲回。 如果輸入不合法的值`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`欄位，`ArgumentException`就會擲回，我們加入程式碼以驗證這些欄位值`ProductsDataTable`類別 (請參閱[建立商務邏輯層](../introduction/creating-a-business-logic-layer-vb.md)教學課程)。

我們可以攔截的例外狀況的類型為基礎的訊息文字，終端使用者提供更有用的說明。 下列程式碼幾乎完全相同的形式使用回[處理 BLL-以及 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)教學課程提供此詳細層級：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

若要完成本教學課程，只要呼叫`DisplayExceptionDetails`方法從`Catch`區塊中攔截到傳遞`Exception`執行個體 (`ex`)。

與`Try ... Catch`就地封鎖，使用者會使用更具資訊性的錯誤訊息，以數字 4 和 5 的顯示。 請注意，遇到例外狀況在 DataList 時處於編輯模式。 這是因為一旦發生例外狀況，控制流程會立即重新導向至`Catch`區塊中，略過的程式碼，DataList 回到前編輯狀態。


[![當使用者遺漏必要的欄位，顯示錯誤訊息](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**圖 4**： 當使用者遺漏必要的欄位，會顯示的錯誤訊息 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![錯誤訊息，顯示時輸入負數價格](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**圖 5**: 錯誤訊息會顯示當輸入是負數的價格 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>總結

GridView 和 ObjectDataSource 提供包含任何在更新和刪除工作流程期間所引發的例外狀況，以及表示已例外狀況可以設定屬性的相關資訊的後置層級的事件處理常式處理。 這些功能，不過，則無法使用時使用 DataList 以及直接使用 BLL。 相反地，我們必須負責實作例外狀況處理。

在此教學課程中我們可了解如何加入例外狀況處理來更新工作流程的加入編輯 DataList s`Try ... Catch`封鎖`UpdateCommand`事件處理常式。 如果在更新的工作流程期間引發例外狀況`Catch`s 區塊程式碼執行時，顯示相關資訊的`ExceptionDetails`標籤。

此時，DataList 完全不會防止在第一次發生的例外狀況。 即使我們知道負價格會導致例外狀況，我們 t 尚未新增任何功能來主動避免使用者輸入此類無效的輸入。 在我們的下一個教學課程中，我們會看到如何有助於減少將驗證控制項中的加入正確的使用者輸入所造成的例外狀況`EditItemTemplate`。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [例外狀況的設計方針](https://msdn.microsoft.com/library/ms298399.aspx)
- [錯誤記錄模組和處理常式 (ELMAH)](http://workspaces.gotdotnet.com/elmah) （記錄錯誤開放原始碼程式庫）
- [Enterprise Library for.NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) （包括例外狀況管理應用程式區塊）

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Ken Pespisa。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](performing-batch-updates-vb.md)
[下一頁](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
