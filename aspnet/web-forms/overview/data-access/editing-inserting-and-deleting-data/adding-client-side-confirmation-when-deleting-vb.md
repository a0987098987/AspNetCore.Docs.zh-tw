---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: 刪除 (VB) 時新增用戶端確認 |Microsoft Docs
author: rick-anderson
description: 在介面中，我們建立了到目前為止，使用者不小心刪除資料時在想要按一下 [編輯] 按鈕，按一下 [刪除] 按鈕。 在此 t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: deae088d1daa63e2936aedf80eded18588b1ec60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830117"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>刪除 (VB) 時新增用戶端確認
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe)或[下載 PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> 在介面中，我們建立了到目前為止，使用者不小心刪除資料時在想要按一下 [編輯] 按鈕，按一下 [刪除] 按鈕。 在本教學課程中，我們將新增用戶端確認對話方塊出現時按一下 [刪除] 按鈕。


## <a name="introduction"></a>簡介

移轉過去的幾個教學課程我們已了解如何搭配使用我們的應用程式架構、 ObjectDataSource 和資料 Web 控制項，來提供插入、 編輯和刪除功能。 刪除介面我們 ve 檢查到目前為止已撰寫的刪除按鈕，當按下時，造成回傳而叫用的 ObjectDataSource s`Delete()`方法。 `Delete()`方法則會叫用的設定的方法，從商業邏輯層，會傳播到資料存取層，發出的實際呼叫`DELETE`到資料庫的陳述式。

雖然此使用者介面可讓訪客，若要刪除的 GridView、 DetailsView 或 FormView 控制項之間的記錄，它缺乏任何一種確認，當使用者按一下 [刪除] 按鈕。 如果使用者不小心按下 [刪除] 按鈕時在想要按一下編輯，在想要更新的記錄將會改為刪除。 為了避免此情形，在本教學課程中，我們將新增用戶端確認對話方塊出現時按一下 [刪除] 按鈕。

JavaScript`confirm(string)`函式會顯示其字串輸入的參數為強制回應對話方塊中，配備兩個按鈕-好 和 取消 （請參閱 圖 1） 內的文字。 `confirm(string)`函式會傳回布林值，根據按下哪個按鈕 (`true`，如果使用者按一下 [確定]，和`false`如果按 [取消])。


![JavaScript confirm(string) 方法會顯示強制回應，用戶端 Messagebox](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**圖 1**: JavaScript`confirm(string)`方法會顯示強制回應的用戶端的 Messagebox


在表單提交時，如果值為`false`傳回從用戶端事件處理常式中，則會取消送出表單。 使用這項功能，我們可以有刪除按鈕的用戶端`onclick`事件處理常式的傳回值呼叫`confirm("Are you sure you want to delete this product?")`。 如果使用者按一下 [取消]，`confirm(string)`會傳回 false，因而導致取消送出表單。 具有任何回傳中，刪除贏得 t 產品按下的 [刪除] 按鈕。 不過，使用者按一下 [確定] 確認對話方塊中的，回傳會繼續而每況愈下則產品將會被刪除。 請參閱[使用 JavaScript s`confirm()`方法，以控制表單提交](http://www.webreference.com/programming/javascript/confirm/)如需有關這項技術。

新增必要的用戶端指令碼稍有不同如果使用比使用 CommandField 的範本。 因此，在本教學課程中，我們將看看 FormView 和 GridView 範例。

> [!NOTE]
> 使用用戶端確認技術，討論在本教學課程中，像是假設與支援 JavaScript 的瀏覽器瀏覽您的使用者，並且具有已啟用 JavaScript。 如果其中一個這些假設不是特定的使用者，則為 true，按一下 [刪除] 按鈕將會立即回傳，（不顯示確認訊息方塊）。


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>步驟 1： 建立 FormView 支援刪除

首先新增至 FormView`ConfirmationOnDelete.aspx`頁面中`EditInsertDelete`資料夾中，繫結至可取回產品資訊，透過新 ObjectDataSource`ProductsBLL`類別的`GetProducts()`方法。 也設定 ObjectDataSource 以便`ProductsBLL`類別 s`DeleteProduct(productID)`方法會對應到 ObjectDataSource`Delete()`方法; 請確認插入和更新] 索引標籤下拉式清單設定為 [（無）。 最後，檢查 FormView s 智慧標籤的 啟用分頁核取方塊。

完成這些步驟中之後, 的新 ObjectDataSource s 宣告式標記看起來如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

如過去範例未使用開放式並行存取，請花一點時間來清除 ObjectDataSource 的`OldValuesParameterFormatString`屬性。

因為已繫結至 ObjectDataSource 控制項僅支援 刪除 FormView 的`ItemTemplate`提供只有 刪除 按鈕，缺少的新功能和更新的按鈕。 FormView s 宣告式標記，不過，包含多餘`EditItemTemplate`和`InsertItemTemplate`，其中可以移除。 花點時間自訂`ItemTemplate`這樣就會顯示資料欄位只是產品的子集。 我已設定我的顯示中的產品的名稱`<h3>`標題上方及其供應商和類別目錄的名稱 （以及 [刪除] 按鈕）。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

經過這些變更，我們有功能完整的網頁，可讓使用者透過一個產品一次切換能夠刪除產品，只要按一下 [刪除] 按鈕。 圖 2 顯示進度的螢幕擷取畫面到目前為止透過瀏覽器檢視時。


[![FormView 顯示單一產品的相關資訊](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**圖 2**: FormView 顯示資訊有關單一產品 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>步驟 2: 刪除按鈕用戶端的 onclick 事件從呼叫 confirm(string) 函式

使用建立 FormView，最後一個步驟是設定 [刪除] 按鈕這類，當它 s 按一下訪客，JavaScript`confirm(string)`函式會叫用。 加入按鈕、 LinkButton 或 ImageButton 的用戶端的用戶端指令碼`onclick`事件，即可使用`OnClientClick property`，這是 ASP.NET 2.0 新功能。 因為我們想要的值`confirm(string)`函式傳回時，只要將設定這個屬性： `return confirm('Are you certain that you want to delete this product?');`

這項變更之後刪除 LinkButton s 宣告式語法看起來應該類似：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

S 就是這麼簡單 ！ 圖 3 顯示作用中的這項確認的螢幕擷取畫面。 按一下 [刪除] 按鈕會帶出 [確認] 對話方塊。 如果使用者按一下 [取消]，回傳會取消，並不會刪除產品。 如果，不過，使用者會按一下 [確定]，回傳會繼續與 ObjectDataSource 的`Delete()`叫用方法時，已累積可被刪除的資料庫記錄。

> [!NOTE]
> 將字串傳遞至`confirm(string)`JavaScript 函式以撇號 （而不是引號）。 在 JavaScript 中，字串可以使用任一字元分隔。 我們使用單引號這裡讓的分隔符號字串傳遞至`confirm(string)`不會產生模稜兩可用於分隔符號`OnClientClick`屬性值。


[![確認訊息會立即顯示時按一下 [刪除] 按鈕](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**圖 3**: 確認會立即顯示時按一下 [刪除] 按鈕 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>步驟 3： 設定 CommandField 中的 [刪除] 按鈕 OnClientClick 屬性

當使用按鈕、 LinkButton、 ImageButton 直接在範本中，確認對話方塊中可以是與它相關聯藉由直接設定其`OnClientClick`屬性，以傳回結果的 JavaScript`confirm(string)`函式。 不過，CommandField-這將刪除按鈕的欄位加入到 GridView 或 DetailsView-沒有`OnClientClick`可以宣告方式設定的屬性。 相反地，我們必須以程式設計方式參考適當的 GridView 或 DetailsView s 中的 [刪除] 按鈕`DataBound`事件處理常式，然後將設定其`OnClientClick`那里屬性。

> [!NOTE]
> 設定 s 上的 [刪除] 按鈕時`OnClientClick`中的適當屬性`DataBound`事件處理常式中，我們可以存取的資料繫結至目前的記錄。 這表示我們可以擴充確認訊息包含有關該特定的記錄，詳細資料，例如，「 是您確定要刪除 Chai 產品嗎？ 」 這種自訂，也可以使用資料繫結語法的範本中。


做法是設定`OnClientClick`刪除 button(s) CommandField，讓 s 中的屬性加入至頁面的 GridView。 設定此 GridView，以使用相同的 ObjectDataSource 控制項，會使用 FormView。 也限制 GridView 的 BoundFields，使其只包含 產品名稱、 類別和供應商。 最後，核取方塊啟用刪除從 GridView s 智慧標籤。 這會將 CommandField 新增到 GridView`Columns`與集合及其`ShowDeleteButton`屬性設定為`true`。

進行這些變更之後，GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

CommandField 包含單一的刪除 LinkButton 執行個體可從 GridView s 以程式設計方式存取`RowDataBound`事件處理常式。 一旦參考，我們可以設定其`OnClientClick`屬性據此。 建立事件處理常式`RowDataBound`使用下列程式碼的事件：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

這個事件處理常式會與資料列 （其會將擁有 [刪除] 按鈕），並一開始會以程式設計方式參考 [刪除] 按鈕。 在一般使用以下模式：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType*是正由 CommandField-LinkButton 或 ImageButton 按鈕的按鈕類型。 根據預設，CommandField 使用 Linkbutton，但這可以自訂經由 CommandField 的`ButtonType property`。 *CommandFieldIndex*是循序索引內的 GridView s CommandField`Columns`集合，而*controlIndex* CommandField s 內的 [刪除] 按鈕的索引`Controls`集合。 *ControlIndex*值取決於 CommandField 相對於其他按鈕的按鈕的位置。 比方說，如果唯一 CommandField 中顯示的按鈕是 [刪除] 按鈕，使用索引為 0。 如果，不過，有 編輯 按鈕之前刪除 按鈕，使用的索引 2。 使用索引為 2 的原因是由 [刪除] 按鈕之前 CommandField 所加入的兩個控制項: [編輯] 按鈕和 LiteralControl s 用來新增一些編輯和刪除按鈕之間的空間。

特定範例中，CommandField 使用 Linkbutton 而且，在最左邊 欄位中，具有*commandFieldIndex*為 0。 因為沒有任何其他按鈕，但在 CommandField 中的 [刪除] 按鈕，我們會使用*controlIndex*為 0。

在參考 CommandField 中的 [刪除] 按鈕之後, 我們接下來會抓取繫結至目前的 GridView 資料列的產品的相關資訊。 最後，我們將設定 [刪除] 按鈕的`OnClientClick`適當的 javascript 中，包括產品的名稱的屬性。 因為 JavaScript 字串傳遞至`confirm(string)`函式使用我們必須將出現在產品的名稱的任何單引號逸出的單引號分隔。 以逸出，s 產品名稱中的任何單引號是特別的是，「`\'`"。

完成這些變更，按一下 刪除 按鈕，在 GridView 會顯示自訂的確認對話方塊 （請參閱 圖 4）。 為和確認訊息方塊，從 FormView，如果使用者按一下 [取消] 回傳被取消，藉此防止發生刪除。

> [!NOTE]
> 這項技術也可用來以程式設計方式存取在 DetailsView 中 CommandField 中的 [刪除] 按鈕。 DetailsView，不過，d 您建立的事件處理常式`DataBound`事件，因為沒有 DetailsView`RowDataBound`事件。


[![按一下 GridView s [刪除] 按鈕會顯示自訂的確認對話方塊](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**圖 4**： 按一下 GridView s [刪除] 按鈕會顯示自訂的確認對話方塊中 ([按一下以檢視完整大小的影像](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>使用 TemplateFields

其中一個 CommandField 的缺點是它的按鈕都必須透過編製索引和產生的物件，必須轉換成適當的按鈕類型 （按鈕、 LinkButton 或 ImageButton）。 使用 「 神奇號碼 」 和硬式編碼的型別邀請找不到執行階段之前的問題。 例如，如果您或另一個開發人員，將新按鈕加入至某個時間點以後 （例如 [編輯] 按鈕），或變更 CommandField`ButtonType`屬性，而沒有錯誤，仍會編譯現有的程式碼，但瀏覽頁面可能會造成例外狀況或非預期的行為，根據您的程式碼撰寫的方式和所做的變更。

替代方式是將 GridView 和 DetailsView 的 CommandFields TemplateFields 轉換。 這會產生使用 TemplateField `ItemTemplate` CommandField 中每個按鈕具有 LinkButton （按鈕或 ImageButton）。 這些按鈕`OnClientClick`屬性可指定以宣告方式，當我們看到了 FormView，或以程式設計的方式可在適當`DataBound`事件處理常式，使用下列模式：


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

何處*controlID*是 [s] 按鈕的值`ID`屬性。 雖然此模式仍然需要硬式編碼的類型轉換的它會移除需要編製索引，以便進行版面配置，以變更不會導致執行階段錯誤。

## <a name="summary"></a>總結

JavaScript`confirm(string)`函式是控制表單提交工作流程的常用的技巧。 執行時，此函式會顯示強制回應的用戶端 對話方塊，其中包含兩個按鈕的 確定 和 取消。 如果使用者按一下 [確定]`confirm(string)`函式會傳回`true`; 按一下 [取消] 會傳回`false`。 這項功能，加上的瀏覽器的行為，可取消表單送出，如果事件處理常式在提交程序期間傳回`false`，可用來刪除記錄時顯示確認訊息方塊。

`confirm(string)`函式可以與按鈕 Web 控制項的用戶端相關聯`onclick`事件處理常式，透過控制的`OnClientClick`屬性。 使用範本-可能是其中一個 FormView 的範本中，或在 DetailsView 或 GridView-TemplateField 中的 [刪除] 按鈕時此屬性可以設定以宣告方式或以程式設計的方式，如我們在本教學課程中所見。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](implementing-optimistic-concurrency-vb.md)
> [下一頁](limiting-data-modification-functionality-based-on-the-user-vb.md)
