---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: 實作開放式並行存取使用 SqlDataSource (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程，我們檢閱必要的開放式並行存取控制，然後瀏覽如何實作使用 SqlDataSource 控制項。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e7e81b3f3a54596c033caa2cf75e5e3ec01764c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>使用 SqlDataSource (VB) 實作開放式並行存取
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe)或[下載 PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> 在本教學課程，我們檢閱必要的開放式並行存取控制，然後瀏覽如何實作使用 SqlDataSource 控制項。


## <a name="introduction"></a>簡介

在前述教學課程中我們會檢查如何新增插入、 更新和刪除 SqlDataSource 控制項的功能。 簡單地說，提供這些功能，我們需要指定對應`INSERT`， `UPDATE`，或`DELETE`控制項 s 中的 SQL 陳述式`InsertCommand`， `UpdateCommand`，或`DeleteCommand`搭配適當的屬性中的參數`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。 [設定資料來源精靈 s 進階] 按鈕時可以手動指定這些屬性和集合，提供產生`INSERT`， `UPDATE`，和`DELETE`陳述式核取方塊會自動建立這些陳述式為基礎`SELECT`陳述式。

產生以及`INSERT`， `UPDATE`，和`DELETE`陳述式核取方塊，進階 SQL 產生選項 對話方塊包含使用開放式並行存取選項 （請參閱圖 1）。 有選取時，`WHERE`中自動產生的子句`UPDATE`和`DELETE`陳述式會修改為僅執行更新或刪除如果因為使用者修改基礎資料庫資料的項目 t 上次載入資料到方格內。


![您可以加入的開放式並行存取支援從進階 SQL 產生選項對話方塊](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**圖 1**： 您可以加入的開放式並行存取支援從進階 SQL 產生選項對話方塊


回到[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)我們檢查的開放式並行存取控制，以及如何將它加入至 ObjectDataSource 基本概念的教學課程。 在本教學課程中，我們將潤飾上必要的開放式並行存取控制，然後瀏覽如何實作使用 SqlDataSource。

## <a name="a-recap-of-optimistic-concurrency"></a>複習一下開放式並行存取

Web 應用程式允許多個使用者同時編輯或刪除相同的資料，有一位使用者可能會不小心覆寫另一個 s 變更的可能性。 在[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)教學課程，我會提供下列的範例：

想像一下，Jisun 與 Sam，兩位使用者已同時瀏覽允許訪客更新及刪除產品透過 GridView 控制項的應用程式中的頁面。 兩者的 Chai 中按一下 [編輯] 按鈕，大約在同一時間。 Jisun 變為 Chai 茶杯的產品名稱，然後按一下 [更新] 按鈕。 最後結果就是`UPDATE`陳述式傳送到資料庫，設定*所有*的產品 s 可更新的欄位 (即使 Jisun 只能更新一個欄位， `ProductName`)。 在此時間點，該資料庫必須 Chai 茶杯，類別飲料，供應商山，對此特定產品值。 不過，Sam 的螢幕上 GridView 仍會顯示產品名稱的可編輯的 GridView 資料列中為 Chai。 幾秒後 Jisun 的變更已認可，Sam 更新 「 調味品 」 類別目錄，然後按一下 更新。 這會導致`UPDATE`陳述式傳送至資料庫所設定的產品名稱 Chai，`CategoryID`到對應 「 調味品 」 類別目錄識別碼，等等。 已覆寫 Jisun 的變更為 產品名稱。

圖 2 說明這種互動。


[![當兩個使用者同時更新資料錄那里一位使用者 s s 可能會變更為覆寫其他](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**圖 2**： 當兩個使用者同時更新覆寫其他變更的記錄那里 s 可能一位使用者 s ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


若要避免這種情況下 unfolding，一種[並行控制](http://en.wikipedia.org/wiki/Concurrency_control)必須實作。 [開放式並行存取](http://en.wikipedia.org/wiki/Optimistic_concurrency_control)本教學課程的焦點運作時可能會有並行衝突不時，大部分的這類衝突贏了 t 的時間發生的假設。 因此，如果沒有發生衝突，開放式並行存取控制項只會通知使用者，其變更無法儲存，因為其他使用者已修改相同的資料。

> [!NOTE]
> 它會假設會有許多並行衝突，或如果此類衝突不容忍範圍內的應用程式，然後封閉式並行存取控制可以改用。 若要回頭參考[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)封閉式並行控制的更完整討論的教學課程。


開放式並行存取控制的運作方式是確保更新或刪除的記錄具有相同的值，更新或刪除處理程序啟動時一樣。 比方說，可編輯的 GridView 中的 [編輯] 按鈕時記錄的值是從資料庫讀取和文字方塊和其他 Web 控制項中顯示。 在 GridView 會儲存這些原始值。 更新的版本，使用者可讓她變更，並按一下 [更新] 按鈕之後`UPDATE`用陳述式必須納入考量的原始值加上新的值，並只更新基礎的資料庫記錄，如果原始值的使用者開始編輯是仍在資料庫中的值相同。 圖 3 描寫此順序的事件。


[![Update 或 Delete 才會成功，原始的值必須等於目前的資料庫值](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**圖 3**: For Update 或 Delete succeed，原始的值必須是等於目前的資料庫值 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


有各種方法可以實作開放式並行存取 (請參閱[Peter A.Bromberg](http://peterbromberg.net/) s [Optmistic 並行更新邏輯](http://www.eggheadcafe.com/articles/20050719.asp)，有多種選項簡短查看)。 使用 SqlDataSource （以及 ADO.NET 型別資料集在我們的資料存取層中使用） 的技術擴大`WHERE`子句，以包含所有原始值的比較。 下列`UPDATE`陳述式，例如，更新的名稱和產品的價格只有當目前的資料庫值不等於更新 GridView 中的記錄時所擷取的值。 `@ProductName`和`@UnitPrice`參數包含由使用者所輸入的新值，而`@original_ProductName`和`@original_UnitPrice`包含了原本 GridView 時載入 [編輯] 按鈕已按下的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

我們會看到此教學課程中，啟用開放式並行存取控制與 SqlDataSource 很簡單，只選取核取方塊。

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>步驟 1： 建立 SqlDataSource 支援開放式並行存取

先開啟`OptimisticConcurrency.aspx`頁面`SqlDataSource`資料夾。 將 SqlDataSource 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性`ProductsDataSourceWithOptimisticConcurrency`。 接下來，也可以按一下 設定資料來源中的連結控制項 s 智慧標籤。 在精靈中第一個畫面中，選擇使用`NORTHWINDConnectionString`，按一下 [下一步]。


[![選擇以 NORTHWINDConnectionString 搭配運作](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**圖 4**： 選擇使用`NORTHWINDConnectionString`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


此範例中我們將加入可讓使用者編輯的 GridView`Products`資料表。 因此，從 設定 Select 陳述式畫面中，選擇 `Products`資料表從下拉式清單，然後選取`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行，如圖 5 所示。


[![從 [產品] 資料表傳回 ProductID、 ProductName、 UnitPrice 和已停止的資料行](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**圖 5**： 從`Products`資料表中，傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


挑選之後資料行中，按一下 [進階] 按鈕以顯示 [進階 SQL 產生選項] 對話方塊。 檢查 產生`INSERT`， `UPDATE`，和`DELETE`陳述式並使用開放式並行存取核取方塊，然後按一下 確定 （請參閱上一步圖 1 螢幕擷取畫面）。 完成精靈，依序按一下 [下一步]，然後完成。

完成設定資料來源精靈之後，請花一點時間來檢查所產生的`DeleteCommand`和`UpdateCommand`屬性和`DeleteParameters`和`UpdateParameters`集合。 若要這樣做最簡單的方式是按一下以查看頁面 s 宣告式語法左下角的 [來源] 索引標籤上。 您會發現那里`UpdateCommand`的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

中的七個參數`UpdateParameters`集合：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

同樣地，`DeleteCommand`屬性和`DeleteParameters`集合應該如下所示：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

除了加強`WHERE`子句`UpdateCommand`和`DeleteCommand`屬性 （和個別的參數集合中加入額外的參數），選取 使用樂觀並行選項會調整其他兩個屬性：

- 變更[`ConflictDetection`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx)從`OverwriteChanges`（預設） 至 `CompareAllValues`
- 變更[`OldValuesParameterFormatString`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx)從 {0} （預設值） 的原始\_{0}。

當資料 Web 控制項叫用 SqlDataSource s`Update()`或`Delete()`方法，它會將傳遞的原始值。 如果 SqlDataSource s`ConflictDetection`屬性設定為`CompareAllValues`，這些原始值都會加入至命令。 `OldValuesParameterFormatString`屬性提供對這些原始值參數所使用的命名模式。 設定資料來源精靈會使用原始\_{0} 與名稱中的每個原始參數`UpdateCommand`和`DeleteCommand`屬性和`UpdateParameters`和`DeleteParameters`集合據此。

> [!NOTE]
> 因為我們重新未使用插入功能，SqlDataSource 控制項 s 隨意移除`InsertCommand`屬性和其`InsertParameters`集合。


## <a name="correctly-handlingnullvalues"></a>正確地處理`NULL`值

不幸的是，增強型`UPDATE`和`DELETE`自動設定資料來源精靈使用開放式並行存取時的陳述式產生執行*不*工作包含的記錄與`NULL`值。 若要查看原因，請考慮我們 SqlDataSource 的`UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice`中的資料行`Products`資料表可以有`NULL`值。 如果特定資料錄有`NULL`值`UnitPrice`、`WHERE`子句部分`[UnitPrice] = @original_UnitPrice`將*一律*評估為 False，因為`NULL = NULL`一律會傳回 False。 因此，記錄包含`NULL`值不能編輯或刪除，做為`UPDATE`和`DELETE`陳述式`WHERE`子句贏了 t 傳回来更新或刪除任何資料列。

> [!NOTE]
> 此第一次報告錯誤給 Microsoft 年 6 月的 2004 中[SqlDataSource 會產生不正確的 SQL 陳述式](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937)並 stage 預計在下一版的 ASP.NET 中修正。


若要修正此問題，我們必須手動更新`WHERE`子句中同時`UpdateCommand`和`DeleteCommand`屬性**所有**可以具有的資料行`NULL`值。 一般情況下，變更`[ColumnName] = @original_ColumnName`至：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

這項修改可直接透過宣告式標記，透過從 屬性 視窗的 UpdateQuery 或 DeleteQuery 選項或透過更新和刪除索引標籤的 指定自訂 SQL 陳述式或預存程序選項的設定資料來源精靈。 同樣地，這項修改必須要有所*每*中的資料行`UpdateCommand`和`DeleteCommand`s`WHERE`子句可以包含`NULL`值。

將此套用到我們的範例會產生下列修改`UpdateCommand`和`DeleteCommand`值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>步驟 2： 新增與編輯和刪除選項的 GridView

使用 SqlDataSource，設定為支援開放式並行存取，所有剩下的都只有將資料 Web 控制項加入至頁面，它會利用此並行存取控制。 本教學課程，可讓 s 新增的 GridView 會提供這兩個編輯和刪除功能。 若要達成此目的，將 GridView 拖曳從工具箱拖曳至設計工具和設定其`ID`至`Products`。 從 GridView s 智慧標籤，將它繫結`ProductsDataSourceWithOptimisticConcurrency`SqlDataSource 控制項加入在步驟 1 中。 最後，請檢查從智慧標籤的 [啟用編輯和啟用刪除] 選項。


[![將 GridView 繫結至 SqlDataSource 並啟用編輯和刪除](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**圖 6**： 繫結的 GridView 的 SqlDataSource 和啟用編輯和刪除 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


加入後 GridView，設定其外觀藉由移除`ProductID`BoundField，變更`ProductName`BoundField s`HeaderText`屬性設為產品，並更新`UnitPrice`BoundField 使其`HeaderText`屬性只要價格。 在理想情況下，我們 d 增強編輯介面，以包含如 RequiredFieldValidator`ProductName`值及針對 CompareValidator `UnitPrice` （以確保 s 已正確格式化的數值） 的值。 請參閱[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程，更深入查看自訂 GridView s 中的編輯介面。

> [!NOTE]
> 必須啟用 s 檢視狀態，因為原始的值從 GridView 傳遞至 SqlDataSource GridView 儲存在檢視狀態。


進行這些修改以 GridView 後, GridView 和 SqlDataSource 宣告式標記看起來應該如下所示：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

若要查看開放式並行存取控制，在動作中的，開啟兩個瀏覽器視窗，並載入`OptimisticConcurrency.aspx`頁面中的。 按一下編輯按鈕，在這兩個瀏覽器中的第一個產品。 在瀏覽器，變更產品名稱，按一下 [更新]。 瀏覽器會回傳和 GridView 會傳回其預先編輯模式，以顯示新的產品名稱，剛建立的記錄。

在第二個瀏覽器視窗中，變更的價格 （但保留做為其原始值的產品名稱），按一下 [更新]。 在回傳時，方格會設定為其預先編輯模式，但是價格的變更不會記錄。 第二個瀏覽器會顯示相同的值與第一個新的產品名稱，舊價格。 在第二個瀏覽器視窗中所做的變更將會遺失。 此外，變更將會遺失而不是無訊息模式，因為發生任何例外狀況或訊息，指出並行違規只發生。


[![第二個瀏覽器視窗中的變更已無訊息方式中斷](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**圖 7**： 在第二個瀏覽器視窗以無訊息模式遺失的變更 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


第二個瀏覽器的變更原因未認可的原因，是因為`UPDATE`陳述式的`WHERE`子句篩選掉所有記錄，因此不會影響任何資料列。 可讓查看 s`UPDATE`一次陳述式：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

當第二個瀏覽器視窗更新記錄時，原始的產品名稱中指定`WHERE`子句規定 t 相符項目上現有的產品名稱 （因為它已變更第一個瀏覽器）。 因此，在陳述式`[ProductName] = @original_ProductName`會傳回 False，而`UPDATE`不會影響任何記錄。

> [!NOTE]
> Delete 運作方式相同。 開啟兩個瀏覽器視窗，以開始編輯指定的產品，其中包含一個項目，並儲存其變更。 在儲存之後所做的變更一種瀏覽器中，按一下 [刪除] 按鈕中的其他相同的產品。 由於原始值不要中匹配`DELETE`陳述式的`WHERE`子句，delete 以無訊息模式失敗。


從第二個瀏覽器視窗之後，按一下方格中的 [更新] 按鈕回到前編輯模式中，但其變更將會遺失使用者 s 檢視方塊。 不過，那里 s 其變更 professionals t 黏貼沒有視覺回應。 在理想情況下，如果使用者 s 會失去變更的並行存取違規，我們 d 通知，而且或許保留方格編輯模式。 可讓 s 看看如何完成這項作業。

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>步驟 3： 決定發生並行存取違規時

並行違規拒絕其中一個已做的變更，因為會是很好的並行存取違規發生時警示使用者。 若要提醒使用者，讓的標籤 Web 控制項加入頁面頂端的名為`ConcurrencyViolationMessage`其`Text`屬性會顯示下列訊息： 您嘗試更新或刪除已由其他使用者同時更新一筆記錄。 請檢閱其他使用者的變更再取消復原您的更新或刪除。 設定標籤控制項 s`CssClass`警告，也就是 CSS 類別屬性中定義`Styles.css`會以紅色、 斜體、 粗體和大字型顯示的文字。 最後，設定標籤 s`Visible`和`EnableViewState`屬性`False`。 這樣會隱藏除了其中我們明確設定的回傳標籤其`Visible`屬性`True`。


[![將標籤控制項加入至頁面，顯示警告](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**圖 8**： 將標籤控制項加入至頁面，顯示警告 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


當執行更新或刪除，GridView s`RowUpdated`和`RowDeleted`事件處理常式之後執行要求的更新或刪除其資料來源控制項所引發。 我們可以判斷多少資料列受到這些事件處理常式進行此作業。 零個資料列受到影響，如果我們想要顯示`ConcurrencyViolationMessage`標籤。

建立兩個事件處理常式`RowUpdated`和`RowDeleted`事件並加入下列程式碼：


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

這兩個事件處理常式檢查`e.AffectedRows`屬性和它等於 0，如果設定`ConcurrencyViolationMessage`標籤 s`Visible`屬性`True`。 在`RowUpdated`事件處理常式中，我們也指示停留在編輯模式，藉由設定 GridView`KeepInEditMode`屬性設定為 true。 在此情況下，我們需要重新繫結至資料格，讓其他使用者的資料載入至編輯介面。 這會透過呼叫 GridView 的`DataBind()`方法。

如圖 9 所示，使用這些兩個事件處理常式中，每次發生並行存取違規時，會顯示非常值得注意的訊息。


[![遇到並行違規時顯示一則訊息](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**圖 9**： 遇到並行違規時，就會顯示訊息 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>總結

建立 web 應用程式時，多個並行使用者編輯相同的資料，請務必考慮並行控制選項。 根據預設，ASP.NET Web 控制項的資料和資料來源控制項不會採用任何並行存取控制。 當我們在本教學課程中所見，實作開放式並行存取控制與 SqlDataSource 會相當快速及簡單。 SqlDataSource 會處理大部分的 legwork，針對您加入夾帶`WHERE`子句來自動產生`UPDATE`和`DELETE`但有陳述式會在處理中的少數微妙`NULL`值資料行中所述正確地處理`NULL`值 > 一節。

本教學課程結束時，我們檢驗 SqlDataSource。 我們其餘的教學課程會傳回處理使用 ObjectDataSource 和階層式的架構資料。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
