---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: 刪除 (VB) 時新增用戶端確認 |Microsoft 文件
author: rick-anderson
description: 我們建立了到目前為止的介面，使用者不小心刪除資料他們在想要按一下 [編輯] 按鈕時，按一下 [刪除] 按鈕。 在此 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 03ab3f9974bca7c3e08b8d3fa6fd4fc786ebed4d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>刪除 (VB) 時新增用戶端確認
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe)或[下載 PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> 我們建立了到目前為止的介面，使用者不小心刪除資料他們在想要按一下 [編輯] 按鈕時，按一下 [刪除] 按鈕。 在本教學課程中，我們會將新增用戶端確認對話方塊出現時按一下 [刪除] 按鈕。


## <a name="introduction"></a>簡介

在過去幾個教學課程我們已看到如何使用我們的應用程式架構、 ObjectDataSource 和 Web 控制項的資料即會一併提供插入、 編輯和刪除功能。 刪除介面我們 ve 檢查到目前為止已包含刪除的按鈕，當按下時，導致回傳而叫用 ObjectDataSource 的`Delete()`方法。 `Delete()`方法再叫用已設定的方法與商務邏輯層，會傳播到資料存取層，發出的實際呼叫`DELETE`到資料庫的陳述式。

雖然此使用者介面可讓訪客刪除記錄的 GridView、 DetailsView 或在 FormView 的控制項，其欠缺任何種類的確認，當使用者按一下 [刪除] 按鈕。 如果使用者不小心按下 [刪除] 按鈕他們在想要按一下時編輯，他們想要更新的記錄將改為刪除。 為了避免此情形，在本教學課程中，我們會將新增用戶端確認對話方塊出現時按一下 [刪除] 按鈕。

JavaScript`confirm(string)`函式會顯示其字串輸入的參數為強制回應對話方塊，來自配備兩個按鈕的 [確定] 並取消 （請參閱圖 1） 內的文字。 `confirm(string)`函式會傳回布林值，視何種按鈕 (`true`，如果使用者按一下 [確定]，和`false`如果按 [取消])。


![JavaScript confirm(string) 方法顯示強制回應，用戶端 Messagebox](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**圖 1**: JavaScript`confirm(string)`方法會顯示強制回應，用戶端的 Messagebox


如果值為表單提交期間`false`已取消送出表單，則會傳回從用戶端事件處理常式。 使用這項功能，我們可以有刪除按鈕的用戶端`onclick`事件處理常式傳回的值呼叫`confirm("Are you sure you want to delete this product?")`。 如果使用者按一下 [取消] 5d;，`confirm(string)`會傳回 false，因而導致取消送出表單。 無回傳，刪除贏了 t 產品的 [刪除] 按鈕已按下。 不過，使用者按一下 [確定] 確認對話方塊中的，如果回傳仍而每況愈下，而且將會刪除產品。 請參閱[使用 JavaScript s`confirm()`方法，以控制表單送出](http://www.webreference.com/programming/javascript/confirm/)如需有關這項技術。

加入必要的用戶端指令碼會有些許如果使用比使用 CommandField 的範本。 因此，在此教學課程中我們將探討 FormView 」 和 「 GridView 範例。

> [!NOTE]
> 使用用戶端確認技術，像是討論在本教學課程中，假設與支援 JavaScript 的瀏覽器瀏覽您的使用者，而且您有已啟用 JavaScript。 如果其中一個這些假設沒有特定使用者，則為 true，按一下 [刪除] 按鈕將會立即回傳 （不顯示確認訊息方塊）。


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>步驟 1： 建立 FormView 支援刪除

啟動新增 FormView`ConfirmationOnDelete.aspx`頁面`EditInsertDelete`資料夾中，繫結至新的 ObjectDataSource 回提取產品資訊，透過`ProductsBLL`類別的`GetProducts()`方法。 也設定 ObjectDataSource 以便`ProductsBLL`類別 s`DeleteProduct(productID)`方法對應到 ObjectDataSource 的`Delete()`方法; 請確認下拉式清單會設定為 （無） 的 INSERT 和 UPDATE 索引標籤。 最後，檢查 FormView s 智慧標籤的 啟用分頁核取方塊。

完成這些步驟之後, 的新 ObjectDataSource s 宣告式標記看起來如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

如同我們未使用開放式並行存取的過去範例，請花一點時間來清除 ObjectDataSource 的`OldValuesParameterFormatString`屬性。

因為它已繫結至 ObjectDataSource 控制項僅支援 刪除 FormView 的`ItemTemplate`提供只有 刪除 按鈕，缺少的新功能和更新的按鈕。 在 FormView s 宣告式標記，不過，包含多餘`EditItemTemplate`和`InsertItemTemplate`，可以移除。 請花一點時間自訂`ItemTemplate`因此也就是資料欄位會顯示產品的子集。 我已設定採擷顯示中的產品的名稱`<h3>`標題上方及其供應商和類別目錄的名稱 （以及 [刪除] 按鈕）。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

這些變更，我們有完全正常運作的網頁，可讓使用者透過產品一個切換一次，只要按一下 [刪除] 按鈕刪除產品的能力。 圖 2 為止顯示進度的螢幕擷取畫面，透過瀏覽器檢視時。


[![在 FormView 顯示單一產品的相關資訊](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**圖 2**: FormView 顯示資訊有關單一產品 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>步驟 2： 從刪除按鈕用戶端 onclick 事件呼叫 confirm(string) 函式

在 FormView 建立時，使用最後一個步驟是設定 [刪除] 按鈕這類，當它 s 按一下訪客，JavaScript`confirm(string)`函式會叫用。 用戶端指令碼加入按鈕、 LinkButton 或 ImageButton 的用戶端`onclick`事件即可使用`OnClientClick property`，這是為 ASP.NET 2.0 的新功能。 因為我們想要的數值`confirm(string)`函式傳回時，只需將此屬性設定： `return confirm('Are you certain that you want to delete this product?');`

這項變更後刪除 LinkButton s 宣告式語法看起來應該像這樣：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

S 都是這麼簡單 ！ 圖 3 顯示此確認的螢幕擷取畫面中的動作。 按一下 [刪除] 按鈕會顯示確認對話方塊。 如果使用者按一下 [取消] 5d;，回傳會取消，並不會刪除產品。 如果，不過，使用者按一下 [確定]，繼續回傳和 ObjectDataSource 的`Delete()`叫用方法時，正在刪除的資料庫記錄中累積。

> [!NOTE]
> 將字串傳遞至`confirm(string)`JavaScript 函式以單引號 （而非引號） 分隔。 在 JavaScript 中，字串可以使用任一字元分隔。 我們使用單引號這裡這樣的分隔符號字串傳遞至`confirm(string)`並不會引入模稜兩可使用的分隔符號用於`OnClientClick`屬性值。


[![確認訊息會立即顯示時按一下 [刪除] 按鈕](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**圖 3**: 確認會立即顯示時按一下 [刪除] 按鈕 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>步驟 3： 在 CommandField 中設定 [刪除] 按鈕 OnClientClick 屬性

使用時按鈕、 LinkButton 或 imagebutton 已直接在範本中，確認對話方塊中可以有與其相關聯只要設定其`OnClientClick`屬性，以傳回結果的 JavaScript`confirm(string)`函式。 不過，CommandField-這會將刪除按鈕的欄位加入 GridView 或 DetailsView-沒有`OnClientClick`可以宣告方式設定的屬性。 相反地，我們必須以程式設計方式參考 [刪除] 按鈕，在適當的 GridView 或 DetailsView s`DataBound`事件處理常式，並將其設定其`OnClientClick`那里屬性。

> [!NOTE]
> 設定 [刪除] 按鈕 s 時`OnClientClick`中適當屬性`DataBound`事件處理常式，都可存取的資料繫結至目前的記錄。 這表示，就可以將確認訊息来包含有關特定的記錄詳細資料，例如，「 是您確定要刪除 Chai 產品嗎？ 」 這類自訂，也可以使用資料繫結語法的範本中。


做法是設定`OnClientClick`刪除 button(s) CommandField 時，可讓 s 中的屬性加入至頁面的 GridView。 設定要使用相同的 ObjectDataSource 控制項 FormView 使用這個 GridView。 也會限制為僅包含產品的名稱、 類別和供應商 BoundFields GridView s。 最後，請檢查 GridView s 智慧標籤啟用刪除核取方塊。 這會新增 CommandField GridView s`Columns`集合，其`ShowDeleteButton`屬性設定為`true`。

進行這些變更之後，GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

CommandField 包含單一的刪除 LinkButton 執行個體可以透過程式設計方式存取從 GridView 的`RowDataBound`事件處理常式。 一旦參考，我們可以設定其`OnClientClick`屬性據此。 建立事件處理常式`RowDataBound`使用下列程式碼的事件：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

這個事件處理常式的資料列 （其將會有 [刪除] 按鈕） 搭配運作，且一開始會以程式設計方式參考 [刪除] 按鈕。 在一般使用以下模式：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType*是正由 CommandField-按鈕、 LinkButton 或 ImageButton 按鈕的型別。 根據預設，CommandField 會使用 LinkButtons，但您可以自訂這個經由 CommandField 的`ButtonType property`。 *CommandFieldIndex*是序數索引內 GridView 的 CommandField`Columns`集合，而*controlIndex* CommandField s 內的 [刪除] 按鈕的索引`Controls`集合。 *ControlIndex*值取決於按鈕的位置相對於其他按鈕 CommandField 中。 例如，如果只有 CommandField 中顯示的按鈕 [刪除] 按鈕，使用索引為 0。 如果，不過，沒有位於 [刪除] 按鈕，[編輯] 按鈕使用索引為 2。 使用索引為 2 的原因是因為兩個控制項所新增的 [刪除] 按鈕前 CommandField: [編輯] 按鈕和 LiteralControl s 用於某些之間加入空格編輯和刪除按鈕。

特定範例中，CommandField 使用 LinkButtons 並，正在最左邊 欄位中，具有*commandFieldIndex*為 0。 因為沒有任何其他按鈕但 CommandField 中的 [刪除] 按鈕，所以我們使用*controlIndex*為 0。

在參考 CommandField 中的 [刪除] 按鈕之後, 我們接下來會抓取繫結至目前的 GridView 資料列的產品的相關資訊。 最後，設定 [刪除] 按鈕的`OnClientClick`適當的 javascript 中，包括產品的名稱的屬性。 因為 JavaScript 字串傳遞至`confirm(string)`函式使用我們必須逸出任何出現在產品的名稱的所有格符號所有格符號分隔。 特別是，在產品的名稱中的任何單引號逸出與 「`\'`"。

完成這些變更，按一下自訂的確認對話方塊 （請參閱圖 4） 的 GridView 會顯示在 [刪除] 按鈕。 做為與確認 messagebox 來自 FormView 中，如果使用者按一下 [取消] 5d; 回傳已取消，因此可以防止發生刪除。

> [!NOTE]
> 這項技術也可用來以程式設計方式存取 CommandField 在 DetailsView 中的 刪除 按鈕。 為 DetailsView，不過，您 d 建立事件處理常式`DataBound`事件，因為在 DetailsView 沒有`RowDataBound`事件。


[![按一下 [GridView 的刪除] 按鈕會顯示自訂的確認對話方塊](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**圖 4**： 按一下 GridView 的刪除 按鈕會顯示自訂確認對話方塊 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>使用 TemplateFields

其中一個 CommandField 的缺點是它的按鈕都必須透過索引而產生的物件必須可以轉換成適當的按鈕類型 （按鈕、 LinkButton 或 ImageButton）。 使用 「 神奇號碼 」 和硬式編碼類型邀請不到執行階段才發現的問題。 例如，如果您或其他開發人員，將新按鈕加入至某個時間點以後 （例如 [編輯] 按鈕），或變更 CommandField`ButtonType`屬性，而沒有錯誤，仍將編譯現有的程式碼，但是瀏覽頁面可能會造成例外狀況或非預期的行為，取決於您的程式碼撰寫的方式和所做的變更。

另一個方法是將 GridView 和 DetailsView 的 CommandFields TemplateFields 轉換。 這會產生具有為 TemplateField `ItemTemplate` CommandField 中每個按鈕具有 LinkButton （按鈕或 ImageButton）。 這些按鈕`OnClientClick`屬性可以指派以宣告方式，如我們看到與 FormView，或以程式設計方式存取在適當`DataBound`事件處理常式使用下列模式：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

其中*controlID*按鈕 s 值`ID`屬性。 雖然這種模式仍需要硬式編碼的類型轉換的它將不再需要編製索引，以便進行變更，而導致執行階段錯誤的配置。

## <a name="summary"></a>總結

JavaScript`confirm(string)`函式是控制表單送出工作流程的常用的技術。 在執行時，函式會顯示強制回應，用戶端 對話方塊，其中包含兩個按鈕的 確定 和 取消。 如果使用者按一下 [確定]，`confirm(string)`函式會傳回`true`; 按一下 [取消] 5d; 傳回`false`。 這項功能，結合其他瀏覽器 s 的行為，如果要取消送出表單送出程序期間的事件處理常式傳回`false`，可用來刪除記錄時顯示確認訊息方塊。

`confirm(string)`函式可以是按鈕 Web 控制項的用戶端相關聯`onclick`事件處理常式透過控制項的`OnClientClick`屬性。 使用範本-其中一個 FormView 的範本中或在 DetailsView 或 GridView-TemplateField 中的 [刪除] 按鈕時這個屬性可以設定以宣告方式或以程式設計的方式，正如我們所見在本教學課程。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](implementing-optimistic-concurrency-vb.md)
> [下一頁](limiting-data-modification-functionality-based-on-the-user-vb.md)
