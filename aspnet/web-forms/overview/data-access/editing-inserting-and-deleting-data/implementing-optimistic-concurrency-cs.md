---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: "實作開放式並行存取 (C#) |Microsoft 文件"
author: rick-anderson
description: "允許多個使用者編輯資料的 web 應用程式，會有兩位使用者，編輯相同的資料一次的風險。 在此 tutori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 50d02e8da7b7ab489e662b42d8f08ad3a99e66eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="implementing-optimistic-concurrency-c"></a>實作開放式並行存取 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe)或[下載 PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> 允許多個使用者編輯資料的 web 應用程式，會有兩位使用者，編輯相同的資料一次的風險。 在本教學課程中，我們會實作開放式並行存取控制來處理此風險。


## <a name="introduction"></a>簡介

對於 web 應用程式，只允許使用者檢視資料，或是包含只有單一使用者可以修改資料，沒有任何威脅的兩個並行的使用者不小心覆寫彼此的變更。 允許多個使用者更新或刪除資料的 web 應用程式，不過，有可能會與其他並行使用者的相衝突的任一使用者的修改。 沒有任何並行的原則，當兩個使用者同時編輯單一記錄中，認可變更的使用者最後會覆寫第一個所做的變更。

例如，想像一下，Jisun 與 Sam，兩位使用者已同時瀏覽我們允許訪客更新及刪除產品透過 GridView 控制項的應用程式中的頁面。 同時按一下 [編輯] 按鈕，在同一時間附近 GridView。 Jisun 產品名稱變更為"Chai 茶杯"，然後按一下 [更新] 按鈕。 最後結果就是`UPDATE`陳述式傳送到資料庫，設定*所有*的產品的可更新的欄位 (即使 Jisun 只能更新一個欄位， `ProductName`)。 在此時間點，資料庫擁有值"Chai 茶杯，「 飲料，供應商山，對此特定產品的類別目錄。 不過，Sam 的螢幕上 GridView 仍會顯示產品名稱可編輯的 GridView 資料列中為"Chai"。 幾秒後 Jisun 的變更已認可，Sam 更新 「 調味品 」 類別目錄，然後按一下 更新。 這會導致`UPDATE`陳述式傳送至資料庫，設定要在 「 Chai，「 產品名稱`CategoryID`到對應飲料類別目錄識別碼，等等。 已覆寫 Jisun 的變更為 產品名稱。 圖 1 以圖形方式描繪此一系列的事件。


[![當兩個使用者同時更新資料錄那里一位使用者 s s 可能會變更為覆寫其他](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**圖 1**： 當兩個使用者同時更新覆寫其他變更的記錄那里 s 可能一位使用者 s ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image3.png))


同樣地，當兩個使用者會瀏覽的頁面，一位使用者可能在更新記錄，當其他使用者刪除。 或者，當使用者載入一頁和之間當他們按一下 [刪除] 按鈕，另一位使用者可能已修改該記錄的內容。

有三個[並行控制](http://en.wikipedia.org/wiki/Concurrency_control)策略：

- **不執行任何動作**-如果使用者同時修改相同的記錄，讓上次認可 win （預設行為）
- [**開放式並行存取**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -假設，可能有並行衝突，不時大部分的情況下也不會發生這類衝突; 因此，如果沒有發生衝突，只是通知使用者，其變更無法儲存，因為其他使用者已修改相同的資料
- **封閉式並行存取**-假設並行衝突很常見，而使用者將不容許是告訴他們變更沒有儲存由於另一位使用者並行活動; 因此，當一位使用者啟動更新記錄時，鎖定藉此防止任何其他使用者編輯或刪除該記錄，直到使用者認可的修改

所有教學課程到目前為止已使用預設並行解決策略-也就是，我們已經讓 win 最後一次寫入。 在本教學課程中，我們將檢驗如何實作開放式並行存取控制。

> [!NOTE]
> 我們將探討封閉式並行存取在此教學課程系列的範例。 封閉式並行存取很少使用，例如鎖定，因為如果不正確時，釋放記憶體，可以防止其他使用者更新資料。 比方說，如果使用者鎖定記錄進行編輯，然後等待解除鎖定前一天，沒有其他使用者將可以更新該記錄，直到傳回原始的使用者，並完成他的更新。 因此，在其中使用封閉式並行存取的情況下，則通常為逾時，如果達到，取消鎖定。 票證銷售網站，當使用者完成訂單程序時，請的短期間鎖定特定的座位位置，都是封閉式並行存取控制項的範例。


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>查看如何開放式並行存取實作步驟 1:

開放式並行存取控制的運作方式是確保更新或刪除的記錄具有相同的值，更新或刪除處理程序啟動時一樣。 比方說，可編輯的 GridView 中的 [編輯] 按鈕時記錄的值會從資料庫讀取和文字方塊和其他 Web 控制項中顯示。 在 GridView 會儲存這些原始值。 更新版本中，使用者可讓她變更，並按一下 [更新] 按鈕之後，原始值加上新的值會傳送到商務邏輯層，然後向資料存取層。 資料存取層必須發出使用者開始編輯的原始值是否仍在資料庫中的值相同，則只會更新記錄的 SQL 陳述式。 圖 2 說明此順序的事件。


[![Update 或 Delete 才會成功，原始的值必須等於目前的資料庫值](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**圖 2**: For Update 或 Delete succeed，原始的值必須是等於目前的資料庫值 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image6.png))


有各種方法可以實作開放式並行存取 (請參閱[Peter A.Bromberg](http://peterbromberg.net/)的[Optmistic 並行更新邏輯](http://www.eggheadcafe.com/articles/20050719.asp)，有多種選項簡短查看)。 ADO.NET 型別資料集提供一個可以設定的核取方塊的刻度的實作。 啟用開放式並行存取的輸入資料集中的 TableAdapter 擴大 TableAdapter 的`UPDATE`和`DELETE`陳述式，以包含所有中的原始值的比較`WHERE`子句。 下列`UPDATE`陳述式，例如，更新的名稱和產品的價格只有當目前的資料庫值不等於更新 GridView 中的記錄時所擷取的值。 `@ProductName`和`@UnitPrice`參數包含由使用者所輸入的新值，而`@original_ProductName`和`@original_UnitPrice`包含了原本 GridView 時載入 [編輯] 按鈕已按下的值：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> 這`UPDATE`陳述式已簡化為提高可讀性。 在實務上，`UnitPrice`簽入`WHERE`子句會更為複雜，因為`UnitPrice`可以包含`NULL`s，正在檢查`NULL = NULL`一律會傳回 False (您必須改用`IS NULL`)。


除了使用不同的基礎`UPDATE`陳述式中，設定使用開放式並行存取也會修改其 DB 的簽章 TableAdapter 直接的方法。 從第一個教學課程中，請記得[*建立資料存取層*](../introduction/creating-a-data-access-layer-cs.md)，DB 直接的方法是那些可接受一份純量值作為輸入的參數 (而不是強型別 DataRow 或DataTable 執行個體）。 當使用開放式並行存取，直接 DB`Update()`和`Delete()`方法包括原始值的輸入的參數。 此外，BLL 使用批次中的程式碼更新模式 (`Update()`接受 Datarow 和 Datatable，而不是純量值的方法多載) 也必須變更。

而是比延伸我們現有 DAL 的 Tableadapter，若要使用的開放式並行存取 （這需要變更以容納 BLL），讓我們改為建立新輸入的資料集名稱為`NorthwindOptimisticConcurrency`，我們將新增`Products`TableAdapter，會使用開放式並行存取。 接下來，我們將建立`ProductsOptimisticConcurrencyBLL`具有適當的修改，以支援開放式並行存取 DAL 的商務邏輯層類別。 一旦此奠定已配置，我們就可以建立 ASP.NET 頁面。

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>步驟 2： 建立資料存取層，支援開放式並行存取

若要建立新的輸入資料集，以滑鼠右鍵按一下`DAL`資料夾內`App_Code`資料夾並加入新的資料集，名為`NorthwindOptimisticConcurrency`。 如我們所見第一個教學課程中，這樣會增加新的 TableAdapter 具類型資料集，會自動啟動 TableAdapter 組態精靈。 在第一個畫面中，我們會提示您指定要連接至-連線到在相同的 Northwind 資料庫使用的資料庫`NORTHWNDConnectionString`從設定`Web.config`。


[![連接到相同的 Northwind 資料庫](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**圖 3**： 連接至相同的 Northwind 資料庫 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image9.png))


接下來，我們會提示您，來選擇查詢的資料： 透過臨機操作 SQL 陳述式中，新的預存程序，或現有的預存程序。 因為我們原始 DAL 中使用特定 SQL 查詢，使用此選項這裡以及。


[![指定要使用特定 SQL 陳述式擷取的資料](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**圖 4**： 指定要使用的特定 SQL 陳述式擷取的資料 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image12.png))


在下列畫面上，輸入要用來擷取產品資訊的 SQL 查詢。 讓我們使用完全相同的 SQL 查詢用於`Products`從傳回的所有我們原始 DAL TableAdapter`Product`以及產品的供應商和類別名稱的資料行：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![在原始的 DAL 中使用相同的 SQL 查詢從產品 TableAdapter](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**圖 5**： 使用相同的 SQL 查詢從`Products`TableAdapter 中原始的 DAL ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image15.png))


再移到下一個畫面上，按一下 [進階選項] 按鈕。 若要讓此 TableAdapter 採用開放式並行存取控制，直接核取 [使用開放式並行存取] 核取方塊。


[![啟用所檢查的開放式並行存取控制&quot;使用開放式並行存取&quot;核取方塊](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**圖 6**： 藉由檢查 「 使用開放式並行存取 」 的核取方塊啟用開放式並行存取控制 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image18.png))


最後，表示 TableAdapter 應該使用的資料存取模式，填滿 DataTable 和傳回 DataTable;也表示應建立這些資料庫直接的方法。 傳回的方法名稱 DataTable 模式變更從 GetData GetProducts，以便在鏡像中我們原始 DAL 我們使用的命名慣例。


[![已利用所有的資料存取模式的 TableAdapter](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**圖 7**： 已將 TableAdapter 利用所有資料存取模式 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image21.png))


完成精靈之後，在 DataSet 設計工具會包含強型別`Products`DataTable 和 TableAdapter。 請花一點時間來重新命名的 DataTable`Products`至`ProductsOptimisticConcurrency`，可執行 DataTable 的標題列上按一下滑鼠右鍵，然後從內容功能表選擇重新命名。


[![DataTable 和 TableAdapter 加入至具類型資料集](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**圖 8**: DataTable 和 TableAdapter 已新增至輸入資料集 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image24.png))


若要查看差異`UPDATE`和`DELETE`查詢之間`ProductsOptimisticConcurrency`TableAdapter （這會使用開放式並行存取） 和產品 TableAdapter （這不會），在 TableAdapter 上按一下，並移至 [屬性] 視窗。 在`DeleteCommand`和`UpdateCommand`屬性`CommandText`子屬性，您可以查看實際叫用 DAL 的更新或刪除相關的方法時，傳送至資料庫的 SQL 語法。 如`ProductsOptimisticConcurrency`TableAdapter`DELETE`陳述式使用：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

而`DELETE`產品 TableAdapter 中我們原始 DAL 的陳述式會更簡單：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

如您所見，`WHERE`中的子句`DELETE`使用開放式並行存取 tableadapter 的陳述式包含比較每個`Product`資料表的現有資料行的值和原始值上次擴展 GridView （或 DetailsView 或 FormView）。 以外的所有欄位後`ProductID`， `ProductName`，和`Discontinued`可以有`NULL`檢查、 額外的參數和值是以正確地比較`NULL`值`WHERE`子句。

我們將不會將加入任何其他 Datatable 至開放式並行存取啟用資料集在此教學課程中，為我們的 ASP.NET 頁面只會提供更新及刪除產品資訊。 不過，我們仍需要加入`GetProductByProductID(productID)`方法`ProductsOptimisticConcurrency`TableAdapter。

若要完成此動作，以滑鼠右鍵按一下 TableAdapter 的標題列上 (上方區域右邊`Fill`和`GetProducts`方法名稱)，然後從內容功能表選擇加入查詢。 這會啟動 [TableAdapter 查詢組態精靈]。 因為我們 TableAdapter 的初始設定中，選擇建立`GetProductByProductID(productID)`方法使用的特定 SQL 陳述式 （請參閱圖 4）。 因為`GetProductByProductID(productID)`方法會傳回特定產品的相關資訊，指出此查詢是`SELECT`查詢傳回的資料列的類型。


[![將查詢類型做為標記&quot;選取會傳回資料列&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**圖 9**： 將標記查詢類型為"`SELECT`傳回資料列的 「 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image27.png))


在下一個畫面我們將會提示您使用，以預先載入的 TableAdapter 的預設查詢的 SQL 查詢。 擴充現有的查詢包含子句`WHERE ProductID = @ProductID`，在圖 10 所示。


[![加入 WHERE 子句加入預先載入的查詢，以傳回特定產品的記錄](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**圖 10**： 新增`WHERE`Pre-Loaded 查詢以傳回特定產品記錄的子句 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image30.png))


最後，變更產生的方法名稱以`FillByProductID`和`GetProductByProductID`。


[![FillByProductID 和 GetProductByProductID 重新命名方法](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**圖 11**： 重新命名的方法`FillByProductID`和`GetProductByProductID`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image33.png))


完成此精靈時，使用 TableAdapter 現在包含兩個方法來擷取資料： `GetProducts()`，它會傳回*所有*產品; 和`GetProductByProductID(productID)`，它會傳回指定的產品。

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>步驟 3： 建立商務邏輯層為開放式並行存取啟用 DAL

我們現有`ProductsBLL`類別具有使用的批次更新 」 和 「 DB 直接模式的範例。 `AddProduct`方法和`UpdateProduct`這兩個的多載使用批次更新模式中，傳入`ProductRow`TableAdapter 的 Update 方法的執行個體。 `DeleteProduct`方法，相反地，使用資料庫的直接模式，呼叫 TableAdapter 的`Delete(productID)`方法。

與新`ProductsOptimisticConcurrency`TableAdapter、 DB 直接方法現在需要在也傳遞的原始值。 例如，`Delete`方法現在預期十個輸入參數： 原始`ProductID`， `ProductName`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`。 它會使用這些額外的輸入的參數的值在`WHERE`子句`DELETE`送出至資料庫，只刪除指定的記錄，如果資料庫的目前值對應到原始的陳述式。

而方法簽章的 TableAdapter 的`Update`尚未變更批次更新模式中所使用的方法，來記錄的原始和新值所需的程式碼。 因此，而不是嘗試使用與我們現有的開放式並行存取啟用 DAL`ProductsBLL`類別，讓我們來建立新的商務邏輯層類別使用我們的新 DAL 的。

將類別命名為`ProductsOptimisticConcurrencyBLL`至`BLL`資料夾內`App_Code`資料夾。


![將 ProductsOptimisticConcurrencyBLL 類別加入 BLL 資料夾](implementing-optimistic-concurrency-cs/_static/image34.png)

**圖 12**： 新增`ProductsOptimisticConcurrencyBLL`BLL 資料夾類別


接下來，加入下列程式碼加入`ProductsOptimisticConcurrencyBLL`類別：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

請注意使用`NorthwindOptimisticConcurrencyTableAdapters`類別宣告的開頭上方的陳述式。 `NorthwindOptimisticConcurrencyTableAdapters`命名空間包含`ProductsOptimisticConcurrencyTableAdapter`類別，可提供 DAL 的方法。 也在類別宣告之前找到`System.ComponentModel.DataObject`會指示 Visual Studio ObjectDataSource 精靈的下拉式清單中包含此類別的屬性。

`ProductsOptimisticConcurrencyBLL`的`Adapter`屬性提供的執行個體的快速存取`ProductsOptimisticConcurrencyTableAdapter`類別，並遵循原始 BLL 類別所使用的模式 (`ProductsBLL`，`CategoriesBLL`等等)。 最後，`GetProducts()`方法只會呼叫向下到 DAL`GetProducts()`方法並傳回`ProductsOptimisticConcurrencyDataTable`物件填入`ProductsOptimisticConcurrencyRow`資料庫中每個產品記錄的執行個體。

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>刪除資料庫直接模式使用開放式並行存取的產品

當使用針對會使用開放式並行存取 DAL 的 DB 直接模式，方法必須傳遞全新和原始值。 用於刪除，沒有任何新值，因此需要傳入原始的值。 在我們 BLL，接著，我們必須接受所有的原始參數做為輸入參數。 讓我們`DeleteProduct`方法中的`ProductsOptimisticConcurrencyBLL`類別使用的 DB 直接的方法。 這表示此方法必須接受所有的十個產品資料欄位，做為輸入參數，而且傳遞這些 dal，如下列程式碼所示：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

如果原始值-這些上次載入的 GridView （或 DetailsView 或 FormView） 的值-當使用者按一下 [刪除] 按鈕在資料庫中的值不同`WHERE`子句不會比對與任何資料庫的記錄和任何記錄會受到影響。 因此，TableAdapter 的`Delete`方法會傳回`0`和 BLL`DeleteProduct`方法會傳回`false`。

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>更新批次更新模式使用開放式並行存取的產品

如前文所述，TableAdapter 的`Update`批次更新模式的方法有相同的方法簽章，不論是否使用開放式並行存取。 也就是`Update`方法預期資料列，資料行、 資料表或輸入資料集的陣列。 有其他任何輸入的參數指定的原始值。 這可能是因為資料表會追蹤的原始和修改過的值為其 DataRow(s)。 當發出 DAL 其`UPDATE`陳述式，`@original_ColumnName`參數會填入 DataRow 的原始值，而`@ColumnName`參數會填入 DataRow 的已修改的值。

在`ProductsBLL`（這會使用我們原始，非開放式並行存取 DAL） 的類別，當使用批次更新模式更新產品資訊的程式碼會執行以下一連串事件：

1. 讀取目前的資料庫產品資訊到`ProductRow`執行個體使用的 TableAdapter`GetProductByProductID(productID)`方法
2. 指派新值來`ProductRow`步驟 1 中的執行個體
3. 呼叫 TableAdapter 的`Update`方法，傳入`ProductRow`執行個體

這一系列的步驟，不過，將不會正確支援開放式並行存取，因為`ProductRow`填入中步驟 1 會填入直接從資料庫中，這表示使用 DataRow 的原始值指目前存在於資料庫，以及未繫結至 GridView 編輯程序的開頭。 相反地，當使用開放式並行存取啟用 DAL，我們需要 alter`UpdateProduct`方法多載，可以使用下列步驟：

1. 讀取目前的資料庫產品資訊到`ProductsOptimisticConcurrencyRow`執行個體使用的 TableAdapter`GetProductByProductID(productID)`方法
2. 指派*原始*值`ProductsOptimisticConcurrencyRow`步驟 1 中的執行個體
3. 呼叫`ProductsOptimisticConcurrencyRow`執行個體的`AcceptChanges()`方法，可指示其目前的值為 「 原始 」 的 DataRow
4. 指派*新*值`ProductsOptimisticConcurrencyRow`執行個體
5. 呼叫 TableAdapter 的`Update`方法，傳入`ProductsOptimisticConcurrencyRow`執行個體

指定的產品記錄的 步驟 1 中的所有目前的資料庫值讀取。 這個步驟是多餘中`UpdateProduct`更新的多載*所有*產品資料行 （做為這些值會覆寫在步驟 2 中），但不可或缺的資料行值的子集當做的傳入其中一個多載輸入的參數。 一旦原始值已指派給`ProductsOptimisticConcurrencyRow`執行個體，`AcceptChanges()`呼叫方法時，這會將目前的 DataRow 值標示為使用中的原始值`@original_ColumnName`中的參數`UPDATE`陳述式。 接下來，將新的參數值指派給`ProductsOptimisticConcurrencyRow`與`Update`叫用方法，傳入 DataRow。

下列程式碼會示範`UpdateProduct`接受所有產品資料的多載欄位作為輸入的參數。 而不顯示在這裡，`ProductsOptimisticConcurrencyBLL`類別包含在下載本教學課程也包含`UpdateProduct`接受只要產品的名稱與價格做為輸入參數的多載。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>步驟 4： 將原始值和新值從 ASP.NET 網頁傳遞至 BLL 方法

DAL 和 BLL 完整，所有剩下的都只有建立 ASP.NET 網頁，可以利用系統內建的開放式並行存取邏輯。 具體而言，Web 控制項 （GridView、 DetailsView 或在 FormView） 的資料必須記住其原始值和 ObjectDataSource 必須將這兩組的值傳遞給商務邏輯層。 此外，必須設定 ASP.NET 網頁，正常處理並行違規。

先開啟`OptimisticConcurrency.aspx`頁面`EditInsertDelete`資料夾，然後將 GridView 加入至設計工具中，設定其`ID`屬性`ProductsGrid`。 從 GridView 的智慧標籤，選擇 建立新的 ObjectDataSource 名為`ProductsOptimisticConcurrencyDataSource`。 因為我們想要使用支援開放式並行存取 DAL 此 ObjectDataSource，將它設定為使用`ProductsOptimisticConcurrencyBLL`物件。


[![具有 ObjectDataSource 使用 ProductsOptimisticConcurrencyBLL 物件](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**圖 13**: ObjectDataSource 使用`ProductsOptimisticConcurrencyBLL`物件 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image37.png))


選擇`GetProducts`， `UpdateProduct`，和`DeleteProduct`精靈中的下拉式清單中的方法。 UpdateProduct 方法，請使用接受所有產品的資料欄位的多載。

## <a name="configuring-the-objectdatasource-controls-properties"></a>設定 ObjectDataSource 控制項的屬性

完成精靈之後，ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

如您所見，`DeleteParameters`集合包含`Parameter`十個輸入參數的每個執行個體`ProductsOptimisticConcurrencyBLL`類別的`DeleteProduct`方法。 同樣地，`UpdateParameters`集合包含`Parameter`每個輸入參數的執行個體`UpdateProduct`。

如需涉及資料修改這些上一個教學課程，我們會移除 ObjectDataSource`OldValuesParameterFormatString`屬性在這個時間點，因為這個屬性會指出 BLL 方法預期的舊 （或原始） 的值，以傳遞，以及新的值。 此外，這個屬性值會指出原始值的輸入的參數名稱。 因為我們會將傳遞的原始值至 BLL，請勿*不*移除這個屬性。

> [!NOTE]
> 值`OldValuesParameterFormatString`屬性必須對應到 BLL 中預期的原始值的輸入的參數名稱。 因為我們命名為這些參數`original_productName`， `original_supplierID`，依此類推，您可以保留`OldValuesParameterFormatString`屬性值當成`original_{0}`。 如果，不過，BLL 方法的輸入的參數具有類似名稱`old_productName`， `old_supplierID`，依此類推，您必須更新`OldValuesParameterFormatString`屬性`old_{0}`。


沒有一個必須是為了讓正確傳遞的原始值 BLL 方法 ObjectDataSource 的最後一個屬性設定。 ObjectDataSource 具有[ConflictDetection 屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx)可以指派給[兩個值的其中一個](https://msdn.microsoft.com/en-US/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`-預設值。不會傳送到 BLL 方法的原始的輸入參數的原始值
- `CompareAllValues`-未傳送的原始值 BLL 方法;選擇此選項，當使用開放式並行存取

請花一點時間設定`ConflictDetection`屬性`CompareAllValues`。

## <a name="configuring-the-gridviews-properties-and-fields"></a>設定 GridView 的屬性和欄位

ObjectDataSource 正確設定的屬性，讓我們來開啟我們注意到設定 GridView。 首先，因為我們想要支援編輯和刪除 GridView，按一下 啟用編輯和刪除啟用核取方塊，從 GridView 的智慧標籤。 這會新增 CommandField 其`ShowEditButton`和`ShowDeleteButton`都設定為`true`。

當繫結至`ProductsOptimisticConcurrencyDataSource`ObjectDataSource，GridView 包含欄位之每個產品的資料欄位。 雖然這類 GridView 可供編輯、 使用者體驗是以外接受的。 `CategoryID`和`SupplierID`BoundFields 會轉譯為文字方塊，要求使用者輸入適當的類別和供應商識別碼。 會有任何格式的數值欄位和任何驗證控制項，以確保產品的名稱已提供且單價庫存單位、 的順序，以及重新訂購層級值的單位是這兩個適當的數值和大於或等於為零。

如我們所述*新增驗證控制項的編輯和插入介面*和*自訂的資料修改介面*教學課程中，可以透過自訂使用者介面取代 TemplateFields BoundFields。 我已修改此 GridView 和其編輯介面，以下列方式：

- 移除`ProductID`， `SupplierName`，和`CategoryName`BoundFields
- 轉換`ProductName`TemplateField 的 BoundField 並加入 RequiredFieldValidation 控制項。
- 轉換`CategoryID`和`SupplierID`TemplateFields 的 BoundFields 和調整使用 DropDownLists，而不是文字方塊的編輯介面。 在這些 TemplateFields' `ItemTemplates`、`CategoryName`和`SupplierName`會顯示資料欄位。
- 轉換`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields TemplateFields 並加入的 CompareValidator 控制項。

因為我們已經已檢查過如何完成這些工作，在先前的教學課程中，我只清單的最後一個宣告式語法這裡，將保留的作法是實作。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

我們非常接近具有完整工作範例。 不過，有幾個微妙不經意向上並讓我們的問題。 此外，我們仍然需要某些介面的並行存取違規時警告使用者。

> [!NOTE]
> 為了讓資料 Web 控制項正確地將原始的值傳給 ObjectDataSource （其接著會傳遞至 BLL），很重要的 GridView`EnableViewState`屬性設定為`true`（預設值）。 如果您停用檢視狀態，不會在回傳時遺失的原始值。


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>將正確的原始值傳遞至 ObjectDataSource

有幾個問題與已設定 GridView 的方式。 如果 ObjectDataSource`ConflictDetection`屬性設定為`CompareAllValues`（現況我們），當 ObjectDataSource`Update()`或`Delete()`GridView （或 DetailsView 或 FormView） 來叫用方法、 ObjectDataSource 嘗試複製GridView 的原始值設成其適當`Parameter`執行個體。 圖 2 回頭參考，如這個程序的圖形表示法。

具體來說，每次資料繫結至 GridView 的雙向資料繫結陳述式中的值會指派 GridView 的原始值。 因此，務必確定所有必要原始值已擷取透過雙向資料繫結和它們所提供的可轉換格式。

若要查看這為何如此重要，請花一點時間瀏覽我們的網頁瀏覽器中。 如預期般，GridView 會列出每個產品具有最左邊的資料行中編輯和刪除按鈕。


[![在 GridView 中所列出的產品](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**圖 14**: 產品會列在 GridView ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image40.png))


如果您按一下 [刪除] 按鈕的任何產品，`FormatException`就會擲回。


[![嘗試刪除任何產品會導致 FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**圖 15**： 嘗試刪除任何產品導致`FormatException`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException`時 ObjectDataSource 嘗試讀取原始引發`UnitPrice`值。 因為`ItemTemplate`具有`UnitPrice`格式化為貨幣 (`<%# Bind("UnitPrice", "{0:C}") %>`)，它包含貨幣符號，例如 $19.95。 `FormatException`發生 ObjectDataSource 會嘗試將此字串插入`decimal`。 若要避免這個問題，我們有許多選項：

- 移除貨幣格式從`ItemTemplate`。 也就是說，而不是使用`<%# Bind("UnitPrice", "{0:C}") %>`，只要使用`<%# Bind("UnitPrice") %>`。 方法的缺點是的價格不會再格式化。
- 顯示`UnitPrice`格式化為貨幣，以在`ItemTemplate`，但使用`Eval`關鍵字來完成這項作業。 請記得，`Eval`執行單向資料繫結。 我們仍需要提供`UnitPrice`原始值，因此我們仍需要雙向資料繫結陳述式中的值`ItemTemplate`，但是這可以放入 Label Web 控制項的`Visible`屬性設定為`false`。 ItemTemplate 中，我們可以使用下列標記：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- 移除貨幣格式從`ItemTemplate`，並使用`<%# Bind("UnitPrice") %>`。 在 GridView 的`RowDataBound`標籤 Web 事件處理常式，以程式設計方式存取控制在其中`UnitPrice`值會顯示，並且設定其`Text`屬性設為格式化的版本。
- 保留`UnitPrice`格式化為貨幣。 在 GridView 的`RowDeleting`事件處理常式，取代現有的原始`UnitPrice`與實際的十進位值使用的值 ($19.95) `Decimal.Parse`。 我們了解如何完成類似`RowUpdating`中的事件處理常式[*處理 BLL-以及 DAL 層級例外狀況，在 ASP.NET 網頁*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程。

選取要使用第二種方法，我例如加入隱藏的標籤 Web 控制項`Text`屬性是雙向資料繫結到未格式化`UnitPrice`值。

之後解決這個問題，請嘗試再次按一下 [刪除] 按鈕的任何產品。 此時，您會得到`InvalidOperationException`ObjectDataSource 嘗試叫用 BLL`UpdateProduct`方法。


[![ObjectDataSource 找不到具有輸入參數，它想要傳送的方法](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**圖 16**: ObjectDataSource 找不到具有輸入參數，它想要傳送的方法 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image46.png))


查看例外狀況的訊息，並明確 ObjectDataSource 想要叫用 BLL`DeleteProduct`方法包含`original_CategoryName`和`original_SupplierName`輸入參數。 這是因為`ItemTemplate`s`CategoryID`和`SupplierID`TemplateFields 目前包含雙向繫結陳述式與`CategoryName`和`SupplierName`資料欄位。 相反地，我們必須包含`Bind`陳述式與`CategoryID`和`SupplierID`資料欄位。 若要完成這項作業，來取代現有繫結陳述式與`Eval`陳述式，然後再加入隱藏的標籤，控制其`Text`屬性繫結至`CategoryID`和`SupplierID`使用雙向資料繫結，如下所示的資料欄位如下：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

這些變更，我們現在已可成功地刪除和編輯產品資訊 ！ 在步驟 5 中我們將探討如何驗證已偵測並行違規。 但現在，在數分鐘再試一次更新和刪除少數的記錄，以確保該更新和刪除為單一使用者可以正常運作。

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>步驟 5： 測試開放式同步存取支援

若要確認正在進行同步存取違規，偵測到 （而非產生盲目覆寫資料中），我們要開啟此頁面的兩個瀏覽器視窗。 在這兩個瀏覽器執行個體中，按一下 [編輯] 按鈕的 Chai。 在其中一個瀏覽器，將名稱變更為"Chai 茶杯"然後按一下 [更新]。 更新應該會成功，並傳回其預先編輯狀態，以做為新的產品名稱的 「 Chai 茶杯"GridView。

其他瀏覽器視窗中執行個體中，不過，產品名稱文字方塊中仍會顯示 「 Chai"。 在此第二個瀏覽器視窗中，更新`UnitPrice`至`25.00`。 沒有開放式並行存取支援，按一下第二個瀏覽器執行個體中的更新會將產品名稱改回"Chai 」，藉此覆寫第一個瀏覽器執行個體所做的變更。 使用採用開放式並行存取，不過，按一下第二個瀏覽器執行個體中的 [更新] 按鈕會產生[DBConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.dbconcurrencyexception.aspx)。


[![DBConcurrencyException 偵測到並行違規時，就會擲回](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**圖 17**： 偵測到時的並行存取違規，`DBConcurrencyException`就會擲回 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException`利用 DAL 的批次更新模式時才會擲回。 DB 直接模式不會引發例外狀況時，它只是指出任何資料列受到影響。 為了說明這點，回到其預先編輯的狀態中的兩個瀏覽器執行個體的 GridView。 接下來，在第一個瀏覽器執行個體中，按一下 [編輯] 按鈕變更回 「 Chai"從"Chai 茶杯 「 產品名稱並按一下 [更新]。 在第二個瀏覽器視窗中，按一下 [刪除] 按鈕的 Chai。

時按一下 [刪除]，在網頁回傳後、 GridView 會叫用 ObjectDataSource`Delete()`方法，以及 ObjectDataSource 呼叫向下`ProductsOptimisticConcurrencyBLL`類別的`DeleteProduct`方法，傳遞的原始值。 原始`ProductName`值的第二個瀏覽器執行個體是 「 Chai 茶杯"，這不符合與目前`ProductName`資料庫中的值。 因此`DELETE`發出至資料庫的陳述式會影響零個資料列，因為在資料庫中沒有任何記錄，`WHERE`滿足子句。 `DeleteProduct`方法會傳回`false`和 ObjectDataSource 資料已不再重新繫結至 GridView。

從使用者的觀點來看，Chai 茶杯第二個瀏覽器視窗中按一下 [刪除] 按鈕上造成螢幕閃爍後返回，, 產品仍然存在，雖然現在已列為 「 Chai 」 （產品名稱所做的變更，第一個瀏覽器執行個體）。 如果使用者再按一下 [刪除] 按鈕，刪除會成功，因為在 GridView 的原始`ProductName`值 ("Chai 」) 現在符合在資料庫中的值。

在這兩種情況下，使用者體驗就遠低於理想。 我們明確不想要顯示的使用者核心`DBConcurrencyException`使用批次更新模式時的例外狀況。 並使用 DB 直接模式時的行為是過程上較為複雜，因為使用者命令失敗，但發生原因不精確指示。

若要補救這兩個問題，我們可以建立更新或刪除失敗的原因，提供更多說明頁面上的標籤 Web 控制項。 對於批次更新模式中，我們可以判斷是否`DBConcurrencyException`視 GridView 的後置層級的事件處理常式，發生例外狀況顯示 [警告] 標籤。 DB 直接方法，我們可以檢查 BLL 方法的傳回值 (也就是`true`一個資料列已受到影響，如果`false`否則)，並視需要顯示告知性訊息。

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>步驟 6： 加入參考訊息和顯示它們遇到並行違規時

並行違規時，固定的行為會取決於是否已使用 DAL 的批次更新或 DB 直接模式。 我們的教學課程會使用兩種模式，與用於更新及刪除所使用的 DB 直接模式所使用的批次更新模式。 若要開始，我們將兩個標籤 Web 控制項加入至我們將說明並行違規發生在嘗試刪除或更新資料時的頁面。 將標籤控制項的`Visible`和`EnableViewState`屬性`false`; 這會使其將會隱藏每個頁面，請造訪的特定頁面瀏覽位置處，在於其`Visible`屬性以程式設計方式設定為`true`。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

除了設定其`Visible`， `EnabledViewState`，和`Text`屬性，也設定`CssClass`屬性`Warning`，因而導致標籤的大型、 紅色、 斜體、 粗體字型顯示。 這個 CSS`Warning`已定義類別，並將其加入 Styles.css 回*檢查插入、 更新和刪除與事件相關聯*教學課程。

加入這些標籤之後，Visual Studio 中的設計師應該類似圖 18。


[![兩個 Label 控制項已加入至頁面](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**圖 18**： 兩個標籤控制項都已加入至頁面 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image52.png))


備妥這些標籤 Web 控制項，我們就可以檢查如何判斷並行違規發生時，這點適當的標籤`Visible`屬性可以設定為`true`，顯示告知性訊息。

## <a name="handling-concurrency-violations-when-updating"></a>更新時，處理並行違規

我們先來看看如何使用批次更新模式時，處理並行違規。 因為這類違規與批次更新模式原因`DBConcurrencyException`例外狀況，我們需要將程式碼加入至我們的 ASP.NET 頁面，來決定是否`DBConcurrencyException`更新程序期間發生例外狀況。 因此，我們應該顯示一個訊息使用者解釋其變更未儲存，因為另一位使用者已修改相同的資料之間，啟動該記錄的編輯時，如果和當他們按一下 [更新] 按鈕。

如我們所見中*處理 BLL-以及 DAL 層級例外狀況，在 ASP.NET 網頁*教學課程，可以偵測並隱藏資料 Web 控制項的後置層級的事件處理常式的這類例外狀況。 因此，我們需要建立事件處理常式的 GridView`RowUpdated`檢查的事件`DBConcurrencyException`擲回例外狀況。 這個事件處理常式會傳遞任何在更新過程中，所引發的例外狀況的參考，事件處理常式程式碼如下所示：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

在 face 的`DBConcurrencyException`例外狀況，此事件處理常式會顯示`UpdateConflictMessage`標籤控制項，並指出已處理此例外狀況。 此位置的程式碼，更新記錄時，就會發生並行存取違規時使用者的變更都會遺失，因為它們會已覆寫其他使用者修改相同的時間。 特別的是，GridView 會傳回其預先編輯狀態，並且繫結至目前的資料庫資料。 這會更新 GridView 資料列與其他使用者的變更，先前不可見的。 此外，`UpdateConflictMessage`標籤控制項將會向使用者解釋發生狀況的短短。 圖 19 會詳細說明此順序的事件。


[![使用者 s 並行存取違規的圖示中不會遺失更新](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**圖 19**: 使用者並行存取違規的圖示中不會遺失更新 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> 或者，而不是傳回 GridView 預先編輯狀態，我們可能會離開 GridView 處於編輯狀態藉由設定`KeepInEditMode`屬性傳入`GridViewUpdatedEventArgs`物件設為 true。 如果您採用這個方法，不過，一定要在重新繫結至 GridView 的資料 (藉由叫用其`DataBind()`方法)，讓其他使用者的值載入至編輯介面。 可供下載與本教學課程中的程式碼具有這兩行程式中的程式碼`RowUpdated`事件處理常式標記為註解，只要取消註解下列幾行程式碼擁有在 GridView 的並行違規之後保持處於編輯模式。


## <a name="responding-to-concurrency-violations-when-deleting"></a>刪除時，回應並行存取違規

DB 直接模式，沒有任何遇到並行違規時引發的例外狀況。 相反地，database 陳述式只會影響任何記錄，做為 WHERE 子句不符合任何記錄。 BLL 中建立的資料修改方法的所有已進行設計，它們會傳回布林值，指出它們受影響的精確一筆記錄。 因此，若要判斷刪除記錄時，是否發生並行存取違規，我們可以檢查傳回值的 BLL`DeleteProduct`方法。

BLL 方法的傳回值可以透過 ObjectDataSource 後續層級的事件處理常式中檢查`ReturnValue`屬性`ObjectDataSourceStatusEventArgs`物件傳遞至事件處理常式。 由於我們有興趣判斷傳回的值從`DeleteProduct`方法中，我們建立所需的事件處理常式 ObjectDataSource`Deleted`事件。 `ReturnValue`屬性屬於型別`object`而且可以是`null`如果引發例外狀況，它無法傳回值之前，已被中斷的方法。 因此，我們應該先確定`ReturnValue`屬性不是`null`是布林值。 假設這項檢查已通過，但我們會示範`DeleteConflictMessage`標籤控制項，如果`ReturnValue`是`false`。 這可以使用下列程式碼來完成：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

面臨並行存取違規，使用者的 delete 要求已取消。 在 GridView 會重新整理，顯示該記錄的時間之間發生使用者的變更載入頁面，且當按下 [刪除] 按鈕。 當這種違規瓿時，`DeleteConflictMessage`顯示標籤，以說明發生狀況的短短 （請參閱圖 20）。


[![S 刪除使用者遇到並行違規時取消](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**圖 20**： 使用者遇到並行違規時已經取消刪除的 s ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>總結

並行違規的機會存在於每個應用程式，可讓多個並行使用者更新或刪除資料。 如果這類違規不計算的當兩個使用者同時更新相同的資料取得 「 最後一個寫入 「 優先 」 中的所有人員都，請覆寫其他使用者的變更。 此外，開發人員可以實作其中一個開放式或封閉式並行控制。 開放式並行存取控制假設並行違規頻繁和只是不允許更新或刪除會構成並行存取違規的命令。 悲觀並行控制會假設是無法接受的並行違規很頻繁，只要拒絕某個使用者的 update 或 delete 命令。 與封閉式並行存取控制項更新資料錄涉及鎖定，藉此防止任何其他使用者修改或刪除記錄鎖定時。

在.NET 中的型別資料集提供支援開放式並行存取控制功能。 特別是，`UPDATE`和`DELETE`發出至資料庫的陳述式包含的所有資料表的資料行，以確保，update 或 delete 才會進行記錄的目前資料類型符合原始資料的使用者應用程式啟動時正在執行的 update 或 delete。 一旦 DAL 已設定為支援開放式並行存取，BLL 方法需要更新。 此外，使得 ObjectDataSource 擷取其資料的 Web 控制項的原始值，並向下傳遞至 BLL 必須設定呼叫 BLL ASP.NET 網頁。

如我們所見本教學課程中，在 ASP.NET web 應用程式中實作開放式並行存取控制涉及更新 DAL 和 BLL 和 ASP.NET 網頁中加入支援。 加入這項工作是不是您的時間和精力明智的抉擇取決於您的應用程式。 如果您不常有並行使用者更新資料，或它們要更新的資料是從另一個不同，則並行控制不是索引鍵的問題。 不過，您定期將多個使用者在您使用的相同資料的站台上，如果並行存取控制有助於防止不知情的狀況下覆寫其他人的一名使用者的更新或刪除。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一頁](customizing-the-data-modification-interface-cs.md)
[下一頁](adding-client-side-confirmation-when-deleting-cs.md)
