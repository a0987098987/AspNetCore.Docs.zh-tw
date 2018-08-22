---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: 處理 BLL 和 DAL 層級的例外狀況 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到如何巧妙地向處理可編輯的 DataList 更新工作流程期間引發的例外狀況。
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: ebaca5ea34fabe3fcd4979eab2e3f684e8e221be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830203"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>處理 BLL 和 DAL 層級的例外狀況 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe)或[下載 PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> 在本教學課程中，我們會看到如何巧妙地向處理可編輯的 DataList 更新工作流程期間引發的例外狀況。


## <a name="introduction"></a>簡介

在 [概觀的編輯和刪除資料 DataList 中](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)教學課程中，我們會建立簡單的編輯和刪除功能形式提供給 DataList。 完整運作，這是不太方便使用，以在編輯期間發生任何錯誤，或刪除處理程序造成未處理的例外狀況。 比方說，省略 s 產品名稱，或當您編輯產品，輸入的價格值非常實惠 ！，則會擲回例外狀況。 程式碼中攔截不到這個例外狀況，因為它會顯示最多 ASP.NET 執行階段，然後在網頁中顯示的 s 的例外狀況詳細資料。

如我們在中所見[處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程中，如果從深海帶上來的商務邏輯或資料存取層級，會引發例外狀況的例外狀況詳細資料都會回到 ObjectDataSource 和然後至 GridView。 我們了解如何依正常程序建立處理這些例外狀況`Updated`或`RowUpdated`ObjectDataSource 或 GridView，檢查例外狀況，以及指出 已處理例外狀況的事件處理常式。

我們 DataList 教學課程中，不過，不 t 使用 ObjectDataSource 的更新和刪除資料。 相反地，我們正努力直接針對 BLL。 若要偵測源自於 BLL 或 DAL 的例外狀況，我們要實作例外狀況處理程式碼後置我們的 ASP.NET 網頁中的程式碼。 在本教學課程中，我們會看到如何更巧妙地向處理可編輯的 DataList s，正在更新工作流程期間引發的例外狀況。

> [!NOTE]
> 在 *概觀的編輯和刪除資料 DataList*更新使用 ObjectDataSource 教學課程中我們討論過編輯和刪除資料 DataList 的不同技術，一些技巧，涉及和正在刪除。 如果您採用這些技術，您可以處理從 DAL 的 BLL 透過 ObjectDataSource s 的例外狀況`Updated`或`Deleted`事件處理常式。


## <a name="step-1-creating-an-editable-datalist"></a>步驟 1： 建立可編輯的資料清單

我們會擔心處理更新的工作流程期間發生的例外狀況之前，讓第一次建立可編輯的 DataList。 開啟`ErrorHandling.aspx`頁面中`EditDeleteDataList`資料夾中，DataList 加入設計工具中，設定其`ID`屬性設`Products`，並新增名為新 ObjectDataSource `ProductsDataSource`。 設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProducts()`選取方法會記錄; 設定下拉式清單中的插入、 更新和刪除索引標籤為 （無）。


[![傳回使用 GetProducts() 方法的產品資訊](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**圖 1**： 傳回產品資訊使用`GetProducts()`方法 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


完成 ObjectDataSource 精靈之後，Visual Studio 會自動建立`ItemTemplate`的 DataList。 將此取代為`ItemTemplate`，顯示每個產品的名稱和價格，並包含 [編輯] 按鈕。 接下來，建立`EditItemTemplate`與 TextBox Web 控制項的名稱和價格和更新 和 取消 按鈕。 最後，設定 DataList 的`RepeatColumns`屬性設為 2。

這些變更之後，請頁面 s 的宣告式標記看起來應該如下所示。 若要進行特定的編輯，也就是 [取消]，請仔細檢查和更新的按鈕有其`CommandName`編輯、 取消，並更新，請分別設定的屬性。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> 在本教學課程 DataList 必須啟用 s 檢視狀態。


請花一點時間檢閱我們的進度，透過瀏覽器 （請參閱 圖 2）。


[![每一個產品包含 [編輯] 按鈕](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**圖 2**： 每一個產品包含 [編輯] 按鈕 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


目前，[編輯] 按鈕只會造成回傳它不 t 尚未讓產品可供編輯。 若要啟用編輯，我們需要建立事件處理常式 DataList s `EditCommand`， `CancelCommand`，和`UpdateCommand`事件。 `EditCommand`並`CancelCommand`事件只會更新 DataList 的`EditItemIndex`屬性和重新繫結至 DataList 的資料：


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

`UpdateCommand`事件處理常式會稍微複雜一點。 它需要讀取已編輯的產品 s`ProductID`從`DataKeys`集合，以及產品的名稱和價格與中的文字方塊`EditItemTemplate`，然後呼叫`ProductsBLL`類別的`UpdateProduct`方法，再傳回 DataList為其預先編輯的狀態。

現在，讓 s 只使用完全相同的程式碼，從`UpdateCommand`中的事件處理常式*概觀的編輯和刪除資料 DataList 中*教學課程。 我們將新增程式碼來依正常程序處理步驟 2 中的例外狀況。


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

發生無效的輸入可以是格式不正確的單價、 不合法的單位價格值會類似-$5.00 或省略的產品名稱，將會引發例外狀況的表單中。 因為`UpdateCommand`事件處理常式不包含任何例外狀況處理程式碼，此時，例外狀況會反昇至 ASP.NET 執行階段，它顯示給使用者 （請參閱 [圖 3]）。


![發生未處理的例外狀況時，使用者會看到錯誤頁面](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**圖 3**： 發生未處理的例外狀況時，使用者會看到錯誤頁面


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>步驟 2： 依正常程序中處理例外狀況的 UpdateCommand 事件處理常式

在更新的工作流程期間發生例外狀況可以`UpdateCommand`DAL、 BLL 或事件處理常式。 比方說，如果使用者輸入的價格為太昂貴`Decimal.Parse`中的陳述式`UpdateCommand`事件處理常式將會擲回`FormatException`例外狀況。 如果在使用者遺漏產品的名稱或價格的值是負數，DAL 會引發例外狀況。

發生例外狀況時，我們想要顯示資訊訊息本身在頁面中。 新增標籤網頁控制項至網頁`ID`設為`ExceptionDetails`。 設定要以紅色顯示，超大型，藉由指定粗體和斜體字型的標籤的文字及其`CssClass`屬性，以`Warning`CSS 類別，其定義於`Styles.css`檔案。

發生錯誤時，我們只想要一次顯示的標籤。 也就是說，在後續回傳時，標籤的警告訊息應該會消失。 這可藉由任一個清除標籤 s`Text`屬性或設定其`Visible`屬性設`False`中`Page_Load`事件處理常式 (如同我們在上一步[處理 BLL 和 DAL 層級在 ASP 中的例外狀況.NET 頁面](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程)，或者停用標籤的檢視狀態的支援。 可讓 s 使用後面這個選項。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

當引發例外狀況時，我們將指派的例外狀況的詳細資料`ExceptionDetails`標籤控制項的`Text`屬性。 因為其檢視狀態已停用，在後續回傳時`Text`屬性 s 以程式設計方式變更將會遺失，再還原至預設文字 （空字串），藉此隱藏的警告訊息。

若要判斷為了幫助的訊息顯示在頁面上引發錯誤，我們需要加入`Try ... Catch`封鎖`UpdateCommand`事件處理常式。 `Try`部分包含程式碼，可能會導致例外狀況，而`Catch`區塊包含在例外狀況下執行的程式碼。 請參閱[例外狀況處理基本概念](https://msdn.microsoft.com/library/2w8f0bss.aspx)一節中的.NET Framework 文件，如需詳細資訊，在`Try ... Catch`區塊。


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

當內的程式碼所擲回任何類型的例外狀況`Try`區塊，`Catch`區塊 s 程式碼會開始執行。 就會擲回的例外狀況的類型`DbException`， `NoNullAllowedException`， `ArgumentException`，並依此類推，取決於內容、 在第一時間導致錯誤。 如果在資料庫層級中，那里的問題`DbException`就會擲回。 如果輸入不合法的值`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`欄位`ArgumentException`將會擲回，我們加入程式碼，以驗證這些欄位值`ProductsDataTable`類別 (請參閱[建立商業邏輯層](../introduction/creating-a-business-logic-layer-cs.md)教學課程)。

我們可以攔截到例外狀況的類型為基礎的訊息文字，使用者提供更有用的說明。 下列程式碼幾乎完全相同的表單中用年代[處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程提供此詳細層級：


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

若要完成本教學課程中，只要呼叫`DisplayExceptionDetails`方法，從`Catch`區塊中攔截到傳遞`Exception`執行個體 (`ex`)。

使用`Try ... Catch`就地區塊中，使用者會使用更具參考性的錯誤訊息，以數字 4 和 5 的顯示。 請注意，面對 DataList 例外狀況會保留在編輯模式。 這是因為在控制流程之後發生的例外狀況，立即重新導向至`Catch`區塊，並略過 DataList 回到預先編輯狀態的程式碼。


[![如果使用者遺漏必要的欄位，就會顯示一則錯誤訊息](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**圖 4**： 如果使用者遺漏必要的欄位，就會顯示錯誤訊息 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![錯誤訊息會顯示當輸入是負的價格](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**圖 5**: 錯誤訊息會顯示當輸入是負的價格 ([按一下以檢視完整大小的影像](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>總結

GridView 和 ObjectDataSource 提供後置的層級的事件處理常式，包括任何更新和刪除工作流程期間所引發的例外狀況，以及可以表示例外狀況已經過設定屬性的相關資訊處理。 這些功能，不過，則無法使用時使用 DataList 與直接使用 BLL。 相反地，我們會負責實作例外狀況處理。

在本教學課程中，我們看到如何新增例外狀況處理來更新工作流程，藉由新增一個可編輯 DataList s`Try ... Catch`封鎖`UpdateCommand`事件處理常式。 如果在更新的工作流程期間引發例外狀況`Catch`區塊 s 程式碼執行時，顯示有用的資訊在`ExceptionDetails`標籤。

此時，DataList 一概不會以防止在第一時間發生例外狀況。 即使我們知道，負數的價格將會導致例外狀況，我們尚未 t 尚未新增任何功能來主動防止使用者輸入這類無效的輸入。 在我們的下一個教學課程中，我們將看到如何協助減少將驗證控制項中的加入無效的使用者輸入所造成的例外狀況`EditItemTemplate`。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [例外狀況的設計方針](https://msdn.microsoft.com/library/ms298399.aspx)
- [錯誤記錄模組和處理常式 (ELMAH)](http://workspaces.gotdotnet.com/elmah) （記錄錯誤的開放原始碼程式庫）
- [Enterprise Library for.NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) （包括例外狀況管理應用程式區塊）

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Ken Pespisa。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](performing-batch-updates-cs.md)
> [下一頁](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
