---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: 實作開放式並行存取以 sqldatasource 進行 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中我們會檢閱開放式並行存取控制的基本知識，並接著探討如何實作使用 SqlDataSource 控制項。
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f2590e8e7712d719eb89403ef839f03066a93d2b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826211"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>實作開放式並行存取以 sqldatasource 進行 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe)或[下載 PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> 在本教學課程中我們會檢閱開放式並行存取控制的基本知識，並接著探討如何實作使用 SqlDataSource 控制項。


## <a name="introduction"></a>簡介

在先前的教學課程中，我們會檢查如何新增插入、 更新和刪除 SqlDataSource 控制項的功能。 簡單地說，若要提供這些功能我們需要指定對應`INSERT`， `UPDATE`，或`DELETE`控制項 s 中的 SQL 陳述式`InsertCommand`， `UpdateCommand`，或`DeleteCommand`搭配適當的屬性中的參數`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。 [設定資料來源精靈 s 進階] 按鈕時可以手動指定這些屬性和集合，提供產生`INSERT`， `UPDATE`，和`DELETE`陳述式核取方塊，將會自動建立這些陳述式為基礎`SELECT`陳述式。

以及產生`INSERT`， `UPDATE`，和`DELETE`陳述式] 核取方塊，[進階 SQL 產生選項] 對話方塊中包含使用開放式並行存取選項 （請參閱 [圖 1）。 有選取時，`WHERE`子句中自動產生`UPDATE`和`DELETE`陳述式會修改為只執行更新或刪除使用者已修改基礎資料庫資料的項目 t 如果上一次資料載入至方格。


![您可以新增開放式並行存取支援從進階 SQL 產生選項對話方塊](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**圖 1**： 您可以新增開放式並行存取支援從進階 SQL 產生選項對話方塊


回到[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教學課程中，我們檢查開放式並行存取控制，以及如何將它新增到 ObjectDataSource 的基本概念。 在本教學課程中，我們將潤飾的開放式並行存取控制 essentials 上，然後探索如何使用 SqlDataSource 實作。

## <a name="a-recap-of-optimistic-concurrency"></a>開放式並行存取的回顧

Web 應用程式的允許多個同時連接的使用者，若要編輯或刪除相同的資料，有一位使用者可能會不小心覆寫另一個 「 s 」 變更的可能性。 在 [實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)我提供下列範例的教學課程：

想像一下，Jisun 和 Sam，兩位使用者已同時瀏覽允許更新及刪除產品透過 GridView 控制項的訪客的應用程式中的頁面。 兩者 Chai 的中按一下 [編輯] 按鈕，大約在同一時間。 Jisun 產品名稱變更為 Chai 茶，然後按一下 [更新] 按鈕。 最後結果就是`UPDATE`傳送至資料庫，可設定的陳述式*所有*的產品 s 可更新的欄位 (即使 Jisun 只更新一個欄位， `ProductName`)。 在此時間點，資料庫會有值 Chai 好茶，準備類別飲料，供應商山，針對這個特定的產品，依此類推。 不過，Sam 的螢幕上的 GridView 仍會顯示產品名稱中可編輯的 GridView 資料列作為 Chai。 幾秒後 Jisun 的變更已認可，Sam 更新 「 調味品 」 類別目錄，然後按一下 更新。 這會導致`UPDATE`陳述式傳送至資料庫的產品名稱設定於 Chai，`CategoryID`對應 「 調味品 」 類別目錄識別碼，等等。 已覆寫 Jisun 的對產品名稱。

[圖 2] 說明這種互動。


[![當兩位使用者同時更新資料錄那里一位使用者 s 可能會變更為覆寫其他](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**圖 2**： 當兩個使用者同時更新那里記錄 s 可能性的一位使用者變更為 覆寫其他 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


若要避免這種情況下展開，一種[並行存取控制](http://en.wikipedia.org/wiki/Concurrency_control)必須實作。 [開放式並行存取](http://en.wikipedia.org/wiki/Optimistic_concurrency_control)時可能會有並行存取衝突不時，大部分的這類衝突贏得 t 時間發生的假設適用於本教學課程的焦點。 因此，如果沒有發生衝突，開放式並行存取控制項只會通知使用者，其變更無法儲存，因為其他使用者已修改相同的資料。

> [!NOTE]
> 它會在此假設會有許多並行衝突，或如果這類衝突是不容忍範圍內的應用程式，然後封閉式並行存取控制可以改用。 回頭[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)封閉式並行存取控制項上的深入討論的教學課程。


開放式並行存取控制的運作方式是確保更新或刪除的記錄有相同的值，更新或刪除處理程序啟動時一樣。 比方說，當按一下可編輯的 GridView 內的 [編輯] 按鈕，記錄的值是從資料庫讀取和文字方塊和其他 Web 控制項中顯示。 GridView 會儲存這些原始值。 更新的版本之後使用者她的變更，然後按一下 [更新] 按鈕，,`UPDATE`陳述式，必須納入考量，原始值加上新的值，並只更新基礎資料庫記錄，如果原始值的使用者開始編輯是仍在資料庫中的值相同。 圖 3 說明這一系列的事件。


[![更新或刪除才會成功，原始的值必須等於目前的資料庫值](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**圖 3**: 更新] 或 [刪除到成功，原始的值必須是等於目前資料庫的值 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


有各種方法來實作開放式同步存取 (請參閱[Peter A.Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic 並行更新邏輯](http://www.eggheadcafe.com/articles/20050719.asp)的幾個選項的簡短探討)。 使用 SqlDataSource （以及 ADO.NET 型別資料集用於我們的資料存取層） 的技術擴大`WHERE`子句，以包含所有的原始值的比較。 下列`UPDATE`陳述式，例如，更新的名稱和產品的價格只有當目前資料庫的值會等於原本擷取更新 GridView 中的記錄時的值。 `@ProductName`並`@UnitPrice`參數會包含由使用者輸入的新值，而`@original_ProductName`和`@original_UnitPrice`包含最初載入 GridView 時按下 [編輯] 按鈕的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

如我們所見本教學課程中，啟用以 sqldatasource 進行的開放式並行存取控制是簡單，只要選取核取方塊。

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>步驟 1： 建立 SqlDataSource 支援開放式並行存取

首先開啟`OptimisticConcurrency.aspx`頁面上，從`SqlDataSource`資料夾。 將 SqlDataSource 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性設`ProductsDataSourceWithOptimisticConcurrency`。 接下來，按一下 從控制項 s 智慧標籤的 設定資料來源 連結。 在精靈中第一個畫面中，選擇要合作`NORTHWINDConnectionString`，按一下 [下一步]。


[![選擇使用 NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**圖 4**： 選擇以處理`NORTHWINDConnectionString`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


此範例中我們會新增可讓使用者編輯的 GridView`Products`資料表。 因此，從 設定 Select 陳述式 畫面中，選擇`Products`資料表從下拉式清單，然後選取`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行，如 圖 5 所示。


[![從 [產品] 資料表中，傳回 ProductID、 ProductName、 UnitPrice 和已停止的資料行](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**圖 5**： 從`Products`資料表中，傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


挑選資料行後, 按一下 [進階] 按鈕以顯示 [進階 SQL 產生選項] 對話方塊。 檢查產生`INSERT`， `UPDATE`，和`DELETE`陳述式並使用開放式並行存取核取方塊，然後按一下 確定 （請參閱上一步 圖 1 提供螢幕擷取畫面）。 完成精靈，依序按一下 [下一步]，然後完成。

完成設定資料來源精靈之後，請花一點時間來檢查產生`DeleteCommand`並`UpdateCommand`屬性並`DeleteParameters`和`UpdateParameters`集合。 若要這樣做最簡單方式是按一下 若要查看頁面 s 宣告式語法左下角的 來源 索引標籤上。 您可以找到`UpdateCommand`的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

中的七個參數`UpdateParameters`集合：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

同樣地，`DeleteCommand`屬性和`DeleteParameters`集合應該如下所示：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

除了增強`WHERE`子句`UpdateCommand`和`DeleteCommand`屬性 （和個別的參數集合中加入額外參數），選取 使用樂觀並行選項會調整其他兩個屬性：

- 變更[`ConflictDetection`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx)從`OverwriteChanges`（預設） 至 `CompareAllValues`
- 變更[`OldValuesParameterFormatString`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx)從{0}（預設值） 到原始影像\_{0} 。

當資料 Web 控制項叫用 SqlDataSource s`Update()`或`Delete()`方法，它會傳入原始值。 如果 SqlDataSource s`ConflictDetection`屬性設定為`CompareAllValues`，這些原始值加入至命令。 `OldValuesParameterFormatString`屬性會提供用於這些原始值參數的命名模式。 設定資料來源精靈會使用原始\_{0}並將在每個原始參數`UpdateCommand`並`DeleteCommand`屬性和`UpdateParameters`和`DeleteParameters`集合據此。

> [!NOTE]
> 因為我們重新不用 SqlDataSource 控制項 s 插入功能，放心地移除`InsertCommand`屬性並將其`InsertParameters`集合。


## <a name="correctly-handlingnullvalues"></a>正確地處理`NULL`值

不幸的是，擴增`UPDATE`並`DELETE`陳述式自動產生設定資料來源精靈使用開放式並行存取時所執行*不*使用包含記錄`NULL`值。 若要瞭解原因，請考慮我們 SqlDataSource 的`UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice`中的資料行`Products`資料表可以有`NULL`值。 如果有特定的資料錄`NULL`值`UnitPrice`，則`WHERE`子句部分`[UnitPrice] = @original_UnitPrice`會*一律*評估為 False，因為`NULL = NULL`一律會傳回 False。 因此，記錄，包含`NULL`值無法編輯或刪除，作為`UPDATE`並`DELETE`陳述式`WHERE`子句就贏了 t 傳回時，若要更新或刪除任何資料列。

> [!NOTE]
> 這個 bug 給 Microsoft 在年 6 月的 2004 年中第一次報告已[SqlDataSource 會產生不正確的 SQL 陳述式](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937)並 stage 排定為在 ASP.NET 中的下一個版本中修正。


若要修正此問題，我們必須手動更新`WHERE`子句中同時`UpdateCommand`並`DeleteCommand`屬性**所有**可以具有的資料行`NULL`值。 一般情況下，變更`[ColumnName] = @original_ColumnName`來：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

這項修改可直接透過宣告式標記，透過 UpdateQuery 或 DeleteQuery 選項，從 屬性 視窗中，或更新和自訂 SQL 陳述式或預存程序選項的設定資料中，刪除在指定的索引標籤來源精靈。 同樣地，這項修改都必須修改*每隔*中的資料行`UpdateCommand`並`DeleteCommand`s`WHERE`子句可以包含`NULL`值。

將此套用到我們的範例會產生下列修改`UpdateCommand`和`DeleteCommand`值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>步驟 2： 新增與編輯和刪除選項 GridView

使用 SqlDataSource 設定為支援開放式並行存取，全都是利用這個並行存取控制的頁面中加入資料 Web 控制項。 本教學課程中，可讓 s 新增的 GridView 會提供這兩個編輯和刪除功能。 若要達成此目的，將 GridView 拖曳從工具箱拖曳至設計工具和設定其`ID`至`Products`。 從 GridView s 智慧標籤，將它繫結`ProductsDataSourceWithOptimisticConcurrency`SqlDataSource 控制項加入在步驟 1 中。 最後，檢查從智慧標籤的 [啟用編輯和啟用刪除] 選項。


[![將 GridView 與 sqldatasource 繫結並啟用編輯和刪除](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**圖 6**： 繫結 GridView SqlDataSource 和啟用編輯和刪除 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


新增 GridView 之後，請藉由移除設定其外觀`ProductID`BoundField 變更`ProductName`BoundField s`HeaderText`屬性設為產品，並更新`UnitPrice`BoundField，讓其`HeaderText`屬性只要價格。 在理想情況下，我們的 d 增強編輯介面，以包含如 RequiredFieldValidator`ProductName`值，如 CompareValidator `UnitPrice` （以確保 s 的正確格式的數值） 的值。 請參閱[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)更深入的了解自訂編輯介面的 GridView s 的教學課程。

> [!NOTE]
> 必須啟用 s 檢視狀態，因為原始的值從 GridView 傳遞至 SqlDataSource 是的 GridView 檢視中儲存狀態。


在 GridView 這些修改之後, 的 GridView 和 SqlDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

若要查看作用中的開放式並行存取控制項，請開啟兩個瀏覽器視窗，並載入`OptimisticConcurrency.aspx`中的頁面。 按一下 [編輯] 按鈕，在這兩個瀏覽器中的第一個產品。 在瀏覽器，變更產品名稱，然後按一下 [更新]。 瀏覽器會回傳和 GridView 會傳回其預先編輯模式，其中會顯示新的產品名稱，剛建立的記錄。

在第二個瀏覽器視窗中，變更 （但保留做為其原始值的產品名稱） 的價格，並按一下 [更新]。 在回傳時，方格傳回給其預先編輯模式，但不是會記錄的價格變更。 第二個瀏覽器會顯示新的產品名稱，與舊的價格與第一個相同的值。 第二個瀏覽器視窗中所做的變更都已中斷。 此外，所做的變更都已中斷而無訊息模式，因為沒有任何例外狀況或訊息，指出並行違規只是發生。


[![第二個瀏覽器視窗中的變更會以無訊息方式遺失](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**圖 7**： 在第二個瀏覽器視窗以無訊息方式遺失的變更 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


為什麼未認可的第二個瀏覽器的變更的原因是因為`UPDATE`陳述式的`WHERE`子句篩選出所有的記錄，並因此不會影響任何資料列。 可讓查看的 s`UPDATE`一次陳述式：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

當第二個瀏覽器視窗中更新記錄時，原始的產品名稱中指定`WHERE`子句不 t 相符項目設定現有的產品名稱 （因為它已變更第一個瀏覽器）。 因此，此陳述式`[ProductName] = @original_ProductName`會傳回 False，而`UPDATE`並不會影響任何記錄。

> [!NOTE]
> Delete 運作方式相同。 開啟兩個瀏覽器視窗，開始編輯指定的產品，並儲存其變更。 在儲存之後所做的變更一種瀏覽器中，按一下 其他相同產品的 刪除 按鈕。 由於原始的值 don t 匹配`DELETE`陳述式的`WHERE`子句中，刪除以無訊息模式失敗。


在第二個瀏覽器視窗之後，按一下方格中的 [更新] 按鈕回到前編輯模式中，但其變更將會遺失使用者 s 觀點。 不過，那里 s 其變更嘛 t 堅持沒有視覺回應。 在理想情況下，如果使用者 s 會失去變更的並行存取違規，我們的 d 通知他們，然後或許保留在編輯模式中的 方格。 可讓 s 看看如何完成這項作業。

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>步驟 3： 決定何時發生並行存取違規

因為並行存取違規會拒絕其中一個已做的變更，那就天下太平了並行存取違規發生時警示使用者。 若要提醒使用者，可讓 s 將標籤 Web 控制項新增至名為頁面頂端`ConcurrencyViolationMessage`其`Text`屬性會顯示下列訊息： 您嘗試更新或刪除另一位使用者同時更新一筆記錄。 請檢閱其他使用者的變更再取消復原您的更新或刪除。 設定 Label 控制項 s`CssClass`為 [警告] 是 CSS 類別屬性中定義`Styles.css`會以紅色、 斜體、 粗體和大字型顯示的文字。 最後，設定標籤 s`Visible`並`EnableViewState`屬性，以`false`。 這樣會隱藏除了我們明確設定的回傳的標籤及其`Visible`屬性設`true`。


[![將標籤控制項新增至頁面，顯示警告](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**圖 8**： 將標籤控制項新增至頁面，顯示警告 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


當執行更新或刪除，s 的 GridView`RowUpdated`和`RowDeleted`事件處理常式引發後其資料來源控制項已執行要求的 update 或 delete。 我們可以判斷這些事件處理常式從作業所影響多少資料列。 零個資料列受到影響，如果我們想要顯示`ConcurrencyViolationMessage`標籤。

針對兩者建立事件處理常式`RowUpdated`和`RowDeleted`事件，並新增下列程式碼：


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

這兩個事件處理常式中我們檢查一下`e.AffectedRows`屬性，然後如果它等於 0 時，設定`ConcurrencyViolationMessage`標籤 s`Visible`屬性設`true`。 在 `RowUpdated`事件處理常式中，我們也指示停留在編輯模式中，藉由設定 GridView`KeepInEditMode`屬性設為 true。 在此情況下，我們需要重新繫結至方格的資料，讓其他使用者的資料載入至編輯介面。 這可以藉由呼叫 GridView 的`DataBind()`方法。

如 圖 9 所示，這些兩個事件處理常式中，每次發生並行違規時，會顯示非常值得注意的訊息。


[![並行存取違規時顯示一則訊息](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**圖 9**： 並行違規時，就會顯示訊息 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>總結

建立 web 應用程式時，多個並行使用者編輯相同的資料，請務必考慮並行控制選項。 根據預設，ASP.NET 資料 Web 控制項和資料來源控制項不會採用任何並行存取控制。 如我們所見本教學課程中，實作以 sqldatasource 進行的開放式並行存取控制是相當快速及簡單。 SqlDataSource 會處理大部分的跑腿，針對您新增擴增`WHERE`子句，以自動產生`UPDATE`並`DELETE`陳述式，但是會在處理中的一些微妙之處`NULL`值資料行中所述正確地處理`NULL`值區段。

本教學課程結束時，我們的 sqldatasource 進行的檢查。 我們其餘的教學課程將會回到使用使用 ObjectDataSource 和階層式的架構的資料。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [下一頁](querying-data-with-the-sqldatasource-control-vb.md)
