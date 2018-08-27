---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: 實作開放式同步存取 (VB) |Microsoft Docs
author: rick-anderson
description: Web 應用程式，可讓多位使用者編輯資料，會有兩位使用者，編輯相同的資料在同一時間的風險。 在此 tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: e33e4b401d957f4aa5560193dd8af0e53ca3b631
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826216"
---
<a name="implementing-optimistic-concurrency-vb"></a>實作開放式同步存取 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe)或[下載 PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Web 應用程式，可讓多位使用者編輯資料，會有兩位使用者，編輯相同的資料在同一時間的風險。 在本教學課程中，我們將實作開放式並行存取控制項來處理此風險。


## <a name="introduction"></a>簡介

對於 web 應用程式，只允許使用者檢視資料，或包含只有單一使用者可以修改資料，沒有任何威脅的兩個並行的使用者不小心覆寫他人的變更。 允許多個使用者更新或刪除資料的 web 應用程式，不過，沒有一位使用者的修改與其他並行使用者的相衝突的可能性。 沒有任何並行存取原則後，當兩個使用者同時編輯單一記錄，認可其變更的使用者最後會覆寫第一個所做的變更。

例如，想像一下，Jisun 和 Sam，兩位使用者已同時造訪我們允許更新和刪除透過 GridView 控制項產品的訪客的應用程式中的頁面。 同時按一下大約在同一時間 GridView 內的 [編輯] 按鈕。 Jisun 產品名稱變更為 「 Chai 茶"，然後按一下 [更新] 按鈕。 最後結果就是`UPDATE`傳送至資料庫，可設定的陳述式*所有*的產品的可更新的欄位 (即使 Jisun 只更新一個欄位， `ProductName`)。 在此時間點，資料庫的值"Chai 茶，「 飲料，供應商山，依此類推的此特定產品的類別。 不過，Sam 的畫面上的 GridView 仍會顯示產品名稱的可編輯的 GridView 資料列中為"Chai 」。 幾秒後 Jisun 的變更已認可，Sam 更新 「 調味品 」 類別目錄，然後按一下 更新。 這會導致`UPDATE`陳述式傳送到設定於 「 Chai，「 產品名稱的資料庫`CategoryID`對應飲料類別目錄識別碼，等等。 已覆寫 Jisun 的產品名稱變更。 [圖 1] 以圖形方式描述這一系列的事件。


[![當兩個使用者同時更新記錄沒有 s 可能會覆寫另一位使用者的變更](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**圖 1**： 當兩個使用者同時更新那里記錄的可能一位使用者變更為 覆寫其他人的 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image3.png))


同樣地，當兩個使用者正在瀏覽的頁面，一位使用者可能啓動刪除由其他使用者時，更新一筆記錄。 或者，當使用者載入頁面時和當他們按一下 [刪除] 按鈕，之間另一位使用者可能已修改該記錄的內容。

有三個[並行存取控制](http://en.wikipedia.org/wiki/Concurrency_control)策略：

- **不執行任何動作**-如果使用者同時修改相同的記錄，讓有機會贏得 （預設行為） 的最後一個認可
- [**開放式並行存取**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -假設，可能有並行存取衝突，不時大部分的時間也不會發生這類衝突; 因此，如果沒有發生衝突，直接告知使用者，他們的變更無法儲存，因為其他使用者已修改相同的資料
- **封閉式並行存取**-假設並行存取衝突是老生常談，且使用者不會容許會告訴他們的變更未儲存，因為另一位使用者並行活動; 因此，當一位使用者啟動更新記錄時，將其鎖定藉此防止任何其他使用者編輯或刪除該記錄，直到使用者認可的修改

我們的教學課程的所有目前為止已使用預設並行解決策略-也就是，我們已讓贏得的最後一個寫入。 在本教學課程中，我們將檢驗如何實作開放式並行存取控制。

> [!NOTE]
> 我們不會在本教學課程系列中的封閉式並行存取範例。 封閉式並行存取很少使用，因為這類鎖定時，如果沒有正確 relinquished，可以防止其他使用者更新資料。 比方說，如果使用者鎖定編輯的記錄，然後離開之前解除其鎖定一天，沒有其他使用者將能夠更新該記錄，直到原始的使用者傳回，並完成他的更新。 因此，在其中使用封閉式並行存取的情況下，有通常是，如果達到，便會取消鎖定逾時。 票證銷售網站，使用者完成訂單程序時，鎖定特定的座位位置短期間內，是封閉式並行存取控制項的範例。


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>看看如何在開放式並行存取實作步驟 1:

開放式並行存取控制的運作方式是確保更新或刪除的記錄有相同的值，更新或刪除處理程序啟動時一樣。 比方說，當按一下可編輯的 GridView 內的 [編輯] 按鈕，記錄的值會從資料庫讀取和文字方塊和其他 Web 控制項中顯示。 GridView 會儲存這些原始值。 更新版本中，使用者會進行她的變更，然後按一下 [更新] 按鈕之後，原始值加上新的值會傳送到商務邏輯層，然後再到資料存取層。 資料存取層必須發出使用者開始編輯原始值是否仍在資料庫中的值相同，則只會更新資料錄的 SQL 陳述式。 圖 2 說明這一系列的事件。


[![更新或刪除才會成功，原始的值必須等於目前的資料庫值](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**圖 2**: 更新] 或 [刪除到成功，原始的值必須是等於目前資料庫的值 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image6.png))


有各種方法來實作開放式同步存取 (請參閱[Peter A.Bromberg](http://peterbromberg.net/)的[Optmistic 並行更新邏輯](http://www.eggheadcafe.com/articles/20050719.asp)的幾個選項的簡短探討)。 ADO.NET 型別資料集提供一個可設定的核取方塊刻度的實作。 針對具類型資料集 TableAdapter 擴大 TableAdapter 的啟用開放式並行存取`UPDATE`並`DELETE`陳述式，以包含所有中的原始值的比較`WHERE`子句。 下列`UPDATE`陳述式，例如，更新的名稱和產品的價格只有當目前資料庫的值會等於原本擷取更新 GridView 中的記錄時的值。 `@ProductName`並`@UnitPrice`參數會包含由使用者輸入的新值，而`@original_ProductName`和`@original_UnitPrice`包含最初載入 GridView 時按下 [編輯] 按鈕的值：


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> 這`UPDATE`陳述式已簡化以方便閱讀。 在實務上，`UnitPrice`簽入`WHERE`子句會更為複雜，因為`UnitPrice`可以包含`NULL`s 和檢查如果`NULL = NULL`一律會傳回 False (相反地，您必須使用`IS NULL`)。


除了使用不同的基礎`UPDATE`陳述式中，設定要使用開放式並行存取也會修改其資料庫簽章以 TableAdapter 直接方法。 從第一個教學課程中，您應該記得[*建立資料存取層*](../introduction/creating-a-data-access-layer-cs.md)，DB 直接的方法是指接受一份純量值作為輸入的參數 (而不是強型別 DataRow 或DataTable 執行個體）。 使用開放式並行存取，直接存取資料庫時`Update()`和`Delete()`方法包括原始值的輸入的參數。 此外，使用批次的 BLL 中的程式碼更新模式 (`Update()`接受 Datarow 和 Datatable，而不是純量值的方法多載) 也必須變更。

而非擴充我們現有的 DAL 的 TableAdapters，若要使用的開放式並行存取 （這需要變更以容納 BLL），讓我們改為建立新輸入資料集名為`NorthwindOptimisticConcurrency`，我們將新增`Products`TableAdapter，使用開放式並行存取。 接下來，我們將建立`ProductsOptimisticConcurrencyBLL`具有適當的修改，支援開放式並行存取 DAL 的商務邏輯層類別。 一旦已配置此基礎，我們就能建立 ASP.NET 網頁。

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>步驟 2： 建立資料存取層，支援開放式並行存取

若要建立新的具類型資料集，以滑鼠右鍵按一下`DAL` `App_Code`資料夾，並新增新的資料集，名為`NorthwindOptimisticConcurrency`。 如我們所見第一個教學課程中，這樣會增加新的 TableAdapter 具類型資料集，會自動啟動 [TableAdapter 組態精靈]。 在第一個畫面中，我們系統會提示來指定要連接到-連線至相同的 Northwind 資料庫使用的資料庫`NORTHWNDConnectionString`上設定`Web.config`。


[![連接到相同的 Northwind 資料庫](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**圖 3**： 連接到相同的 Northwind 資料庫 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image9.png))


接下來，我們會提示您，如何查詢資料： 臨機操作 SQL 陳述式，透過新的預存程序，或現有預存程序。 由於我們已在我們原始的 DAL 使用特定 SQL 查詢，使用此選項在此也。


[![指定要使用特定 SQL 陳述式擷取的資料](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**圖 4**： 指定要使用特定 SQL 陳述式擷取的資料 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image12.png))


接下來的畫面中，輸入要用來擷取產品資訊的 SQL 查詢。 讓我們使用完全相同的 SQL 查詢用於`Products`從我們原始的 DAL，會傳回所有的 TableAdapter`Product`以及產品的供應商和類別名稱的資料行：


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![在原始的 DAL 使用相同的 SQL 查詢從產品的 TableAdapter](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**圖 5**： 使用相同的 SQL 查詢，從`Products`TableAdapter 中原始的 DAL ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image15.png))


再移到下一個畫面上，按一下 [進階選項] 按鈕。 此外，要有這個 TableAdapter 採用開放式並行存取控制項，只要核取 [使用開放式並行存取] 核取方塊。


[![啟用所檢查的開放式並行存取控制&quot;使用開放式並行存取&quot;核取方塊](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**圖 6**： 藉由選取 [使用開放式並行存取] 核取方塊啟用開放式並行存取控制 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image18.png))


最後，表示 TableAdapter 應該使用的資料存取模式，同時填入 DataTable，並傳回 DataTable;也表示應建立這些資料庫的直接方法。 將傳回的方法名稱的 DataTable 模式從 GetData GetProducts，以便反映我們在我們原始的 DAL 中使用的命名慣例。


[![已利用所有的資料存取模式的 TableAdapter](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**圖 7**： 具有 TableAdapter 利用所有資料存取模式 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image21.png))


完成精靈之後，DataSet 設計工具將包含強型別`Products`DataTable 和 TableAdapter。 請花一點時間來重新命名從 DataTable`Products`至`ProductsOptimisticConcurrency`，可以完成這件事您 DataTable 的標題列上按一下滑鼠右鍵，然後從操作功能表選擇重新命名。


[![DataTable 和 TableAdapter 已新增至具類型資料集](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**圖 8**: DataTable 和輸入資料集已加入 TableAdapter ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image24.png))


若要查看之間的差異`UPDATE`並`DELETE`查詢之間`ProductsOptimisticConcurrency`TableAdapter （這會使用開放式並行存取） 和產品 TableAdapter （這不），在 TableAdapter 上按一下，然後移至 [屬性] 視窗。 在 `DeleteCommand`並`UpdateCommand`屬性`CommandText`子屬性，您可以看到實際 DAL 的更新或刪除相關的方法叫用時，傳送至資料庫的 SQL 語法。 針對`ProductsOptimisticConcurrency`TableAdapter`DELETE`陳述式使用：


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

而`DELETE`我們原始的 DAL 產品 TableAdapter 的陳述式是更簡單：


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

如您所見，`WHERE`中的子句`DELETE`會使用開放式並行存取 TableAdapter 的陳述式包含每個之間比較`Product`資料表現有資料行的值和原始值時上次擴展 GridView （或 DetailsView 或 FormView）。 因為以外的所有欄位`ProductID`， `ProductName`，並`Discontinued`可以有`NULL`檢查、 額外的參數和值是包含正確比較`NULL`中的值`WHERE`子句。

我們不會新增任何額外的 Datatable 開放式並行存取啟用資料集在此教學課程中，因為我們的 ASP.NET 頁面只會提供更新和刪除產品資訊。 不過，我們仍需要加入`GetProductByProductID(productID)`方法，以`ProductsOptimisticConcurrency`TableAdapter。

若要這麼做，以滑鼠右鍵按一下 TableAdapter 的標題列上 (上方區域右邊`Fill`和`GetProducts`方法名稱)，然後從內容功能表中選擇 加入查詢。 這會啟動 [TableAdapter 查詢組態精靈]。 因為我們 TableAdapter 的初始組態中，選擇建立`GetProductByProductID(productID)`使用特定 SQL 陳述式的方法 （請參閱 圖 4）。 由於`GetProductByProductID(productID)`方法會傳回特定產品的相關資訊，代表這項查詢的`SELECT`查詢傳回的資料列的類型。


[![將查詢類型做為標記&quot;選取會傳回資料列&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**圖 9**： 將查詢類型做為標記 」`SELECT`傳回資料列 」 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image27.png))


在下一個畫面系統會提示我們提供的 SQL 查詢，以搭配預先載入的 TableAdapter 的預設查詢。 加強現有的查詢，以包含子句`WHERE ProductID = @ProductID`，如 [圖 10] 所示。


[![加入 WHERE 子句加入預先載入的查詢，以傳回特定產品的記錄](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**圖 10**： 新增`WHERE`Pre-Loaded 查詢以傳回特定的產品記錄的子句 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image30.png))


最後，變更產生的方法名稱以`FillByProductID`和`GetProductByProductID`。


[![FillByProductID 和 GetProductByProductID 重新命名方法](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**圖 11**： 重新命名的方法`FillByProductID`並`GetProductByProductID`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image33.png))


完成此精靈中，使用 TableAdapter 現在包含兩個方法來擷取資料： `GetProducts()`，就會傳回*所有*產品; 以及`GetProductByProductID(productID)`，它會傳回指定的產品。

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>步驟 3： 為開放式並行存取啟用 DAL 建立商業邏輯層

我們現有`ProductsBLL`類別已使用的批次更新 」 和 「 DB 直接模式的範例。 `AddProduct`方法和`UpdateProduct`多載兩個使用批次更新模式中，傳入`ProductRow`TableAdapter 的 Update 方法的執行個體。 `DeleteProduct`方法，相反地，會使用資料庫的直接存取模式，呼叫 TableAdapter 的`Delete(productID)`方法。

與新`ProductsOptimisticConcurrency`TableAdapter，DB 直接方法現在需要也傳遞中的原始值。 例如，`Delete`方法現在預期有十個輸入參數： 原始`ProductID`， `ProductName`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`。 它會使用這些其他輸入的參數的值，在`WHERE`子句`DELETE`送出至資料庫，只刪除指定的記錄，如果資料庫的目前值對應到原始的陳述式。

而方法簽章的 TableAdapter 的`Update`批次更新模式中所使用的方法尚未變更，記錄的原始和新的值所需的程式碼。 因此，而不是嘗試使用與我們現有的開放式並行存取啟用 DAL`ProductsBLL`類別中，讓我們來建立新的商務邏輯層類別，使用我們新的 DAL。

新增名為類別`ProductsOptimisticConcurrencyBLL`要`BLL`資料夾內`App_Code`資料夾。


![將 ProductsOptimisticConcurrencyBLL 類別新增到 BLL 資料夾](implementing-optimistic-concurrency-vb/_static/image34.png)

**圖 12**： 新增`ProductsOptimisticConcurrencyBLL`BLL 資料夾類別


接下來，新增下列程式碼`ProductsOptimisticConcurrencyBLL`類別：


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

請注意使用`NorthwindOptimisticConcurrencyTableAdapters`類別宣告的開頭上方的陳述式。 `NorthwindOptimisticConcurrencyTableAdapters`命名空間包含`ProductsOptimisticConcurrencyTableAdapter`類別，可提供 DAL 的方法。 也在類別宣告之前您會發現`System.ComponentModel.DataObject`屬性，它會指示 Visual Studio ObjectDataSource 精靈的下拉式清單中包含此類別。

`ProductsOptimisticConcurrencyBLL`的`Adapter`屬性會提供快速存取的執行個體`ProductsOptimisticConcurrencyTableAdapter`類別，並遵循在我們原始的 BLL 類別中使用的模式 (`ProductsBLL`，`CategoriesBLL`等等)。 最後，`GetProducts()`方法會直接呼叫向下 DAL`GetProducts()`方法並傳回`ProductsOptimisticConcurrencyDataTable`物件填入`ProductsOptimisticConcurrencyRow`資料庫中每個產品記錄的執行個體。

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>正在刪除產品，使用開放式並行存取的資料庫直接存取模式

使用針對會使用開放式並行存取的 DAL DB 直接模式時，方法必須傳遞的全新及原始值。 刪除，沒有任何新值，因此需要傳入原始值。 在我們的 BLL，接著，我們必須接受所有的原始參數做為輸入參數。 讓我們`DeleteProduct`方法中的`ProductsOptimisticConcurrencyBLL`類別使用的 DB 直接方法。 這表示此方法必須在所有的十個產品資料欄位，做為輸入參數，並傳給 DAL，如下列程式碼所示：


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

如果原始值-這些上次載入的 GridView （或 DetailsView 或 FormView） 的值-當使用者按一下 [刪除] 按鈕在資料庫中的值不同`WHERE`子句不匹配任何資料庫的記錄和任何記錄將會受到影響。 因此，TableAdapter`Delete`方法會傳回`0`和 BLL`DeleteProduct`方法會傳回`false`。

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>正在更新產品，批次更新模式使用開放式並行存取

如前文所述，TableAdapter 的`Update`批次更新模式的方法有相同的方法簽章，不論是否採用開放式並行存取。 也就是`Update`方法預期的 DataRow，Datarow、 DataTable 或輸入資料集的陣列。 有其他任何輸入的參數指定的原始值。 這可能是因為 DataTable 會持續追蹤的原始和修改過的值為其 DataRow(s)。 當的 DAL 發出其`UPDATE`陳述式中，`@original_ColumnName`參數會填入 DataRow 的原始值，而`@ColumnName`參數會填入 DataRow 的修改過的值。

在  `ProductsBLL` （這會使用我們原始，非開放式並行存取 DAL） 的類別，以更新我們的程式碼會執行以下一連串事件的產品資訊使用批次更新模式時：

1. 讀取到目前的資料庫產品資訊`ProductRow`執行個體使用的 TableAdapter`GetProductByProductID(productID)`方法
2. 指派新值來`ProductRow`步驟 1 中的執行個體
3. 呼叫 TableAdapter`Update`方法並傳入`ProductRow`執行個體

這一系列的步驟，不過，不會正確支援開放式並行存取，因為`ProductRow`填入在步驟 1 是直接從資料庫填入，這表示使用 DataRow 的原始值的目前存在於資料庫中，而不是繫結至 GridView 編輯程序的開頭。 相反地，使用開放式並行存取-啟用時 DAL，我們需要 alter`UpdateProduct`方法多載，可以使用下列步驟：

1. 讀取到目前的資料庫產品資訊`ProductsOptimisticConcurrencyRow`執行個體使用的 TableAdapter`GetProductByProductID(productID)`方法
2. 指派*原始*值來`ProductsOptimisticConcurrencyRow`步驟 1 中的執行個體
3. 呼叫`ProductsOptimisticConcurrencyRow`執行個體的`AcceptChanges()`方法，它會指示 DataRow，其目前值就是所謂 「 原創 」
4. 指派*新*值來`ProductsOptimisticConcurrencyRow`執行個體
5. 呼叫 TableAdapter`Update`方法並傳入`ProductsOptimisticConcurrencyRow`執行個體

指定的產品記錄的 步驟 1 中的所有目前的資料庫值讀取。 這個步驟是在多餘`UpdateProduct`更新的多載*所有*產品資料行 （做為這些值會覆寫在步驟 2 中），但對於其中的資料行值的子集會傳入做為多載是不可或缺輸入的參數。 之後的原始值已指派給`ProductsOptimisticConcurrencyRow`執行個體，請`AcceptChanges()`呼叫方法時，將當做原始值，用於標示目前的 DataRow 值`@original_ColumnName`中的參數`UPDATE`陳述式。 接下來，將新的參數值指派給`ProductsOptimisticConcurrencyRow`以及最後`Update`叫用方法，傳入 DataRow。

下列程式碼示範`UpdateProduct`多載，接受所有的產品資料欄位作為輸入的參數。 不在這裡顯示，而`ProductsOptimisticConcurrencyBLL`類別包含在下載本教學課程也包含`UpdateProduct`接受只是產品的名稱和價格，做為輸入參數的多載。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>步驟 4： 從 ASP.NET 頁面的原始和新值傳遞到 BLL 方法

DAL 和完整的 BLL，全都是建立 ASP.NET 網頁，可以利用內建至系統的開放式並行存取邏輯。 具體來說，資料 Web 控制項 （GridView、 DetailsView 或 FormView） 必須記住其原始值與 ObjectDataSource 必須將這兩組值傳遞給商務邏輯層。 此外，ASP.NET 網頁必須設定為適當地處理 並行違規。

首先開啟`OptimisticConcurrency.aspx`頁面中`EditInsertDelete`資料夾，然後將 GridView 加入至設計工具中，設定其`ID`屬性設`ProductsGrid`。 從 GridView 的智慧標籤上，選擇建立新的 ObjectDataSource 名為`ProductsOptimisticConcurrencyDataSource`。 因為我們希望此 ObjectDataSource 使用 DAL 支援開放式並行存取，將它設定為使用`ProductsOptimisticConcurrencyBLL`物件。


[![ObjectDataSource 使用了 ProductsOptimisticConcurrencyBLL 物件](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**圖 13**： 已將 ObjectDataSource`ProductsOptimisticConcurrencyBLL`物件 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image37.png))


選擇`GetProducts`， `UpdateProduct`，和`DeleteProduct`精靈中的下拉式清單中的方法。 UpdateProduct 方法中，使用多載可接受的所有產品的資料欄位。

## <a name="configuring-the-objectdatasource-controls-properties"></a>設定 ObjectDataSource 控制項的屬性

完成精靈之後，ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

如您所見，`DeleteParameters`集合包含`Parameter`十個輸入參數的每個執行個體`ProductsOptimisticConcurrencyBLL`類別的`DeleteProduct`方法。 同樣地，`UpdateParameters`集合包含`Parameter`每個輸入參數的執行個體`UpdateProduct`。

針對涉及資料修改這些先前的教學課程，我們會移除 ObjectDataSource 的`OldValuesParameterFormatString`屬性在這個時間點，因為這個屬性會指出，BLL 方法預期的舊的 （或原始） 的值，必須傳入，以及新的值。 此外，這個屬性值會指出原始值的輸入的參數名稱。 因為我們會傳入原始值到 BLL，請勿*不*移除這個屬性。

> [!NOTE]
> 值`OldValuesParameterFormatString`屬性必須對應到 BLL 中預期的原始值的輸入的參數名稱。 因為我們命名為這些參數`original_productName`，`original_supplierID`等等，您可以將保留`OldValuesParameterFormatString`屬性值作為`original_{0}`。 如果，不過，BLL 方法的輸入的參數有名稱，例如`old_productName`，`old_supplierID`等等，您需要更新`OldValuesParameterFormatString`屬性設`old_{0}`。


沒有一個需要為了讓將正確的原始值的 BLL 方法的 ObjectDataSource 進行的最後一個屬性設定。 ObjectDataSource 已[ConflictDetection 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx)可以指派給[兩個值之一](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -預設值;不會傳送給 BLL 方法的原始的輸入參數的原始值
- `CompareAllValues` -未傳送的原始值的 BLL 方法;使用開放式並行存取時，請選擇這個選項

請花一點時間來設定`ConflictDetection`屬性設`CompareAllValues`。

## <a name="configuring-the-gridviews-properties-and-fields"></a>設定 GridView 的屬性和欄位

正確設定 ObjectDataSource 的屬性，讓我們把焦點轉到設定 GridView。 首先，因為我們希望 GridView，以支援編輯和刪除時，按一下 從 GridView 的智慧標籤的 啟用編輯和啟用刪除核取方塊。 這會新增 CommandField 其`ShowEditButton`並`ShowDeleteButton`都設為`true`。

當繫結至`ProductsOptimisticConcurrencyDataSource`ObjectDataSource，GridView 包含欄位之每個產品的資料欄位。 雖然這類 GridView 可供編輯、 使用者體驗是任何內容，不過可接受。 `CategoryID`和`SupplierID`BoundFields 會轉譯文字方塊中，為需要使用者識別碼作為輸入適當的類別和供應商。 會有任何格式的數值欄位和任何驗證控制，以確保已提供了產品的名稱，且，單價，庫存單位數、 的順序和重新排列等級值的單位是兩個適當的數值大於或等於為零。

如我們所述*將驗證控制項加入編輯和插入介面*並*自訂資料修改介面*教學課程中，可以藉由自訂使用者介面取代 TemplateFields BoundFields。 我已透過下列方式修改這個 GridView 和其編輯介面：

- 移除`ProductID`， `SupplierName`，和`CategoryName`BoundFields
- 轉換`ProductName`TemplateField 的 BoundField 和加入 RequiredFieldValidation 控制項。
- 轉換`CategoryID`和`SupplierID`BoundFields 至 TemplateFields，並調整使用 dropdownlist 進行，而不是文字方塊的編輯介面。 在這些 TemplateFields' `ItemTemplates`，則`CategoryName`和`SupplierName`會顯示資料欄位。
- 轉換`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields TemplateFields 和新增的 CompareValidator 控制項。

因為我們已經討論過程如何在先前的教學課程完成這些工作，我只是清單的最後一個宣告式語法這裡，將保留的作法是實作。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

我們非常接近具有完整工作範例。 不過，有一些微妙之處，將會向上蔓延並讓我們的問題。 此外，我們仍需要一些發生並行違規時，系統會通知使用者的介面。

> [!NOTE]
> 為了讓資料 Web 控制項將正確的原始值的 ObjectDataSource （其接著會傳遞至 BLL），是重要的 GridView`EnableViewState`屬性設定為`true`（預設值）。 如果您停用檢視狀態，原始的值會遺失在回傳。


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>將正確的原始值傳遞到 ObjectDataSource

有幾個問題與已設定 GridView 的方式。 如果 ObjectDataSource`ConflictDetection`屬性設定為`CompareAllValues`（現況與我們的洞察力），當 ObjectDataSource 的`Update()`或`Delete()`GridView （或 DetailsView 或 FormView） 來叫用方法，ObjectDataSource 會嘗試複製GridView 的原始值設成其適當`Parameter`執行個體。 圖 2 回頭參考，此程序的圖形化表示法。

具體來說，每次資料繫結至 GridView 的雙向資料繫結陳述式中的值會指派 GridView 的原始值。 因此，很重要，擷取所有需要原始值時，會透過雙向資料繫結和，它們會提供轉換的格式。

若要查看這為何重要，請花一點時間瀏覽我們的網頁瀏覽器中。 如預期般，GridView 會列出每項產品，包含最左邊的資料行中編輯和刪除按鈕。


[![在 GridView 中所列出的產品](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**圖 14**: 產品會列在 GridView ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image40.png))


如果您按一下 [刪除] 按鈕，針對任何產品，`FormatException`就會擲回。


[![嘗試刪除 FormatException 中的任何產品結果](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**圖 15**： 嘗試在刪除任何產品結果`FormatException`([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image43.png))


`FormatException` ObjectDataSource 嘗試讀取原始程式碼時，會引發`UnitPrice`值。 由於`ItemTemplate`已經`UnitPrice`格式化為貨幣 (`<%# Bind("UnitPrice", "{0:C}") %>`)，它包含貨幣符號，例如 $19.95。 `FormatException`發生 ObjectDataSource 會嘗試將此字串`decimal`。 若要避免這個問題，我們有一些選項：

- 移除 貨幣格式從`ItemTemplate`。 也就是說，而不是使用`<%# Bind("UnitPrice", "{0:C}") %>`，只要使用`<%# Bind("UnitPrice") %>`。 這個缺點是，不會再格式化價格。
- 顯示`UnitPrice`格式化為貨幣，以在`ItemTemplate`，但使用`Eval`關鍵字來完成這項作業。 請記得，`Eval`執行單向資料繫結。 我們仍必須提供`UnitPrice`原始的值，所以我們仍然需要雙向資料繫結陳述式中的值`ItemTemplate`，但這可以放在標籤 Web 控制項其`Visible`屬性設定為`false`。 ItemTemplate 中，我們可以使用下列標記：


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- 移除格式的貨幣`ItemTemplate`，並使用`<%# Bind("UnitPrice") %>`。 在 GridView `RowDataBound` Label Web 事件處理常式中，以程式設計方式存取控制在其中`UnitPrice`值會顯示，並且設定其`Text`屬性設為格式化的版本。
- 保留`UnitPrice`格式化為貨幣。 在 GridView`RowDeleting`事件處理常式，取代現有的原始`UnitPrice`使用實際的十進位值的值 ($19.95) `Decimal.Parse`。 我們了解如何完成類似的項目`RowUpdating`中的事件處理常式[*處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程。

我範例中，前往第二個方法中，我選擇加入隱藏的 Label Web 控制項`Text`屬性是雙向資料繫結至未格式化`UnitPrice`值。

之後解決這個問題，請嘗試再按一下 任何產品的 刪除 按鈕。 此時，您會收到`InvalidOperationException`ObjectDataSource 嘗試叫用的 BLL`UpdateProduct`方法。


[![ObjectDataSource 找不到具有其想要傳送的輸入參數的方法](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**圖 16**: ObjectDataSource 找不到具有其想要傳送的輸入參數的方法 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image46.png))


查看例外狀況的訊息，很顯然，ObjectDataSource 會想要叫用的 BLL`DeleteProduct`方法，其中包含`original_CategoryName`和`original_SupplierName`輸入參數。 這是因為`ItemTemplate`s`CategoryID`並`SupplierID`TemplateFields 目前包含雙向繫結陳述式與`CategoryName`和`SupplierName`資料欄位。 相反地，我們必須包含`Bind`陳述式與`CategoryID`和`SupplierID`資料欄位。 若要達成此目的，取代 使用現有的繫結陳述式`Eval`陳述式，然後新增 隱藏的標籤控制項`Text`屬性會繫結至`CategoryID`和`SupplierID`使用雙向資料繫結，如所示的資料欄位下面：


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

經過這些變更，我們就能成功地刪除及編輯產品資訊 ！ 步驟 5 中我們將探討如何驗證會在偵測到並行違規。 但現在請花幾分鐘的時間嘗試更新和刪除一些記錄，以確保該更新和刪除針對單一使用者如預期般運作。

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>步驟 5： 測試的開放式並行存取支援

若要確認正在進行並行存取違規，偵測到 （而非產生盲目地覆寫的資料中），我們要開啟此頁面的兩個瀏覽器視窗。 在這兩個瀏覽器執行個體中，按一下 [編輯] 按鈕的 Chai。 然後，只是其中一種瀏覽器，將名稱變更為 「 Chai 茶"，並按一下 [更新]。 更新應該會成功，並傳回其預先編輯的狀態，以做為新的產品名稱的"Chai 茶"GridView。

在其他瀏覽器視窗中執行個體，不過，產品名稱文字方塊中仍會顯示 「 Chai"。 在這個第二個瀏覽器視窗中，更新`UnitPrice`至`25.00`。 沒有開放式並行存取支援，按一下第二個瀏覽器執行個體中的更新會將產品名稱改回"Chai 」，藉此覆寫第一個瀏覽器執行個體所做的變更。 使用採用開放式並行存取，不過，按一下第二個瀏覽器執行個體中的 [更新] 按鈕會導致[DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx)。


[![DBConcurrencyException 偵測到並行違規時，會擲回](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**圖 17**： 偵測時的並行存取違規，則`DBConcurrencyException`就會擲回 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image49.png))


`DBConcurrencyException`利用 DAL 的批次更新模式時才會擲回。 DB 直接模式不會引發例外狀況，只是表示沒有任何資料列受到影響。 為了說明這點，請將這兩個瀏覽器執行個體的 GridView 回到其預先編輯的狀態。 接下來，在第一個瀏覽器執行個體中，按一下 [編輯] 按鈕變更回 「 Chai"從"Chai 茶 」 產品名稱並按一下 [更新]。 在第二個瀏覽器視窗中，按一下 [刪除] 按鈕的 Chai。

時按一下 [刪除] 回傳網頁中，會叫用的 ObjectDataSource 的 GridView`Delete()`方法，與 ObjectDataSource 呼叫向下`ProductsOptimisticConcurrencyBLL`類別的`DeleteProduct`方法，傳遞的原始值。 原始`ProductName`值的第二個瀏覽器執行個體是 「 Chai 茶 」，這目前不符合`ProductName`資料庫中的值。 因此`DELETE`發出給資料庫的陳述式會影響零個資料列，因為在資料庫中沒有記錄，`WHERE`子句滿足。 `DeleteProduct`方法會傳回`false`和 ObjectDataSource 的資料會重新繫結至 GridView。

從使用者的觀點來看，Chai 茶，第二個瀏覽器視窗中按一下 [刪除] 按鈕會造成快閃畫面和時返回，產品仍然存在，但現在它會列為 「 Chai 」 （產品名稱所做的變更，第一個瀏覽器執行個體）。 如果使用者再次按一下 [刪除] 按鈕，刪除會成功，因為 GridView 的原始`ProductName`值 ("Chai 」) 現在個比對資料庫中的值。

在這兩種情況下，使用者體驗遠低於理想。 我們明確不想要向使用者顯示的這些枝微末節`DBConcurrencyException`使用批次更新模式時的例外狀況。 並使用 DB 直接模式時的行為是有點令人困惑，因為使用者的命令失敗，但不會精確指出的原因。

若要解決這兩個問題，我們可以建立標籤 Web 控制項在更新或刪除失敗的原因，請提供說明頁面上。 對於批次更新模式中，我們可以判斷是否`DBConcurrencyException`視在 GridView 的後置的層級事件處理常式中發生例外狀況顯示 [警告] 標籤。 DB 直接方法，我們可以檢查 BLL 方法的傳回值 (即`true`影響了一個資料列，則如果`false`否則)，並視需要顯示告知性訊息。

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>步驟 6： 新增參考用訊息並將它們顯示發生並行存取違規

並行存取違規發生時，固定的行為取決於是否已使用 DAL 的批次更新或 DB 直接模式。 在本教學課程會使用這兩種模式，以用於更新和 DB 直接模式用來刪除批次更新模式。 若要開始，我們將兩個 Label Web 控制項新增至我們說明嘗試刪除或更新的資料時發生並行違規的頁面。 將標籤控制項的`Visible`並`EnableViewState`屬性，以`false`; 這會使其除了對於特定頁面瀏覽的位置，在每個頁面造訪隱藏其`Visible`屬性以程式設計方式設定為`true`。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

除了設定其`Visible`， `EnabledViewState`，並`Text`屬性，我也設定`CssClass`屬性設`Warning`，因而導致標籤要顯示在大型、 紅色、 斜體、 粗體字型中的。 這個 CSS`Warning`已定義類別，並將其加入 Styles.css 年代*檢查與插入、 更新和刪除事件相關聯*教學課程。

之後新增這些標籤，在 Visual Studio 中的設計工具應該看起來會類似 圖 18。


[![兩個 Label 控制項已新增至頁面](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**圖 18**： 兩個標籤控制項都已加入至網頁 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image52.png))


這些標籤 Web 的地方使用控制項，我們已經準備好檢驗如何判斷何時發生並行存取違規，而此時適當的標籤`Visible`屬性可以設定為`true`，顯示參考用訊息。

## <a name="handling-concurrency-violations-when-updating"></a>更新時，處理並行存取違規

讓我們先看看如何使用批次更新模式時，處理並行存取違規。 因為這類違規之批次更新模式的原因`DBConcurrencyException`例外狀況，我們需要將程式碼新增到我們的 ASP.NET 頁面，來決定是否`DBConcurrencyException`更新程序期間所發生的例外狀況。 如果因此，我們應顯示到使用者的說明不儲存其變更，因為另一位使用者已修改相同的資料之間，當他們開始編輯記錄的訊息，當他們按一下 [更新] 按鈕。

如我們在中所見*處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁*教學課程中，可以偵測並於資料 Web 控制項的後置的層級事件處理常式中隱藏這類例外狀況。 因此，我們需要建立事件處理常式，如 GridView`RowUpdated`檢查的事件`DBConcurrencyException`擲回例外狀況。 這個事件處理常式會在更新過程中，引發任何例外狀況的參考，事件處理常式程式碼如下所示：


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

在面對的`DBConcurrencyException`例外狀況，此事件處理常式會顯示`UpdateConflictMessage`標籤控制項，並指出例外狀況已處理。 使用此程式碼就緒之後，更新資料錄，時發生並行違規時使用者的變更都會遺失，因為它們會已覆寫其他使用者修改相同的時間。 特別的是，GridView 會回到其預先編輯的狀態，並繫結至目前的資料庫資料。 這會使用先前不可見的其他使用者的變更來更新 GridView 資料列。 此外， `UpdateConflictMessage` Label 控制項將會向使用者解釋發生狀況。 圖 19 會詳細說明這一系列的事件。


[![使用者 s 中的並行存取違規圖示不會遺失更新](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**圖 19**: 使用者並行存取違規的圖示中不會遺失更新 ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> 或者，而不會傳回 GridView 預先編輯狀態，我們無法將 GridView 處於編輯狀態設定`KeepInEditMode`屬性的傳入`GridViewUpdatedEventArgs`設為 true 的物件。 如果您採用這種方法，不過，一定要在重新繫結至 GridView 資料 (藉由叫用其`DataBind()`方法)，讓其他使用者的值載入至編輯介面。 可供下載本教學課程中的程式碼具有這兩行程式中的程式碼`RowUpdated`事件處理常式加上註解，只要取消註解後並行存取違規這行程式碼有 GridView 仍保留在編輯模式。


## <a name="responding-to-concurrency-violations-when-deleting"></a>刪除時回應並行存取違規

使用 DB 直接模式時，沒有並行違規時引發的例外狀況。 相反地，database 陳述式只會影響任何記錄，做為 WHERE 子句不符合任何記錄。 建立到 BLL 中的資料修改方法的所有已設計，它們會傳回布林值，指出它們受影響的精確地一筆記錄。 因此，若要判斷刪除記錄時，是否發生並行存取違規，我們可以檢查傳回值的 BLL`DeleteProduct`方法。

BLL 方法的傳回值，請檢查透過 ObjectDataSource 的後置的層級的事件處理常式`ReturnValue`屬性`ObjectDataSourceStatusEventArgs`物件傳遞至事件處理常式。 因為我們感興趣判斷傳回的值從`DeleteProduct`方法中，我們需要建立 ObjectDataSource 的事件處理常式`Deleted`事件。 `ReturnValue`屬性的類型是`object`而且可以是`null`如果引發例外狀況，而且之前，它會傳回值，此方法已中斷。 因此，我們應該先確定`ReturnValue`屬性不是`null`是布林值。 假設這項檢查通過，我們會示範`DeleteConflictMessage`標籤控制項，如果`ReturnValue`是`false`。 這可藉由使用下列程式碼：


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

發生並行存取違規，已取消使用者的 delete 要求。 GridView 會重新整理，顯示發生該記錄的時間之間的使用者的變更載入頁面時按下 [刪除] 按鈕。 當這類違規情形瓿時，`DeleteConflictMessage`標籤會顯示說明發生狀況 （請參閱圖 20）。


[![S 刪除的使用者已取消面對並行存取違規](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**圖 20**： 並行違規時取消刪除的使用者 s ([按一下以檢視完整大小的影像](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>總結

並行存取違規的機會存在於每個應用程式，可讓多個並行使用者更新或刪除資料。 如果此違規情況為時兩個使用者同時更新相同的資料取得最後一個寫入 「 獲勝 」 中的任何使用者，會覆寫其他使用者的變更的變更不計算在內。 或者，開發人員可以實作其中一個開放式或封閉式並行存取控制項。 開放式並行存取控制項會假設，並行存取違規不頻繁且只是不允許更新或刪除會構成並行存取違規的命令。 封閉式並行存取控制項會假設該並行違規很頻繁和只要拒絕一位使用者更新或刪除命令不接受。 透過封閉式並行存取控制，更新記錄涉及鎖定，藉此防止任何其他使用者修改或刪除記錄，而它已被鎖定。

輸入資料集，在.NET 中提供支援開放式並行存取控制的功能。 特別是，`UPDATE`和`DELETE`發出給資料庫的陳述式包含的所有資料表的資料行，藉此確保，update 或 delete 才會與原始資料符合記錄的目前資料的使用者應用程式啟動時正在執行其 update 或 delete。 DAL 支援開放式並行存取設定之後, 的 BLL 方法需要更新。 此外，使得 ObjectDataSource 會從其資料 Web 控制項擷取原始的值，而再向下傳遞到 BLL，必須設定 ASP.NET 網頁會呼叫 BLL。

如我們所見本教學課程中，在 ASP.NET web 應用程式中實作開放式並行存取控制項涉及更新 BLL 和 DAL，以及在 ASP.NET 網頁中新增支援。 這個新增的工作是不是明智的抉擇的時間和精力，取決於您的應用程式。 如果您不常有的並行使用者更新資料，或它們要更新的資料不會彼此不同，並行存取控制則不重要的問題。 如果，不過，您定期有多位使用者在您使用的相同資料的站台上，並行存取控制可協助防止一位使用者更新或刪除，因而不自覺地覆寫另一個的。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](customizing-the-data-modification-interface-vb.md)
> [下一頁](adding-client-side-confirmation-when-deleting-vb.md)
