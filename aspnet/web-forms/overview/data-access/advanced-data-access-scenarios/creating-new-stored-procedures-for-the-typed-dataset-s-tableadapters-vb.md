---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: 建立新的預存程序的具類型資料集的 Tableadapter (VB) |Microsoft 文件
author: rick-anderson
description: 在先前的教學課程中我們有我們的程式碼中建立 SQL 陳述式，傳遞至資料庫，才能執行這些陳述式。 另一個方法是使用 s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 3df3623f5575a48a22fb1a2c3bc719975a80b9c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877758"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>建立新的預存程序的具類型資料集的 Tableadapter (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip)或[下載 PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> 在先前的教學課程中我們有我們的程式碼中建立 SQL 陳述式，傳遞至資料庫，才能執行這些陳述式。 另一個方法是使用預存程序，其中的 SQL 陳述式會在資料庫中預先定義。 本教學課程中我們了解如何讓 TableAdapter 精靈為我們產生新的預存程序。


## <a name="introduction"></a>簡介

這些教學課程中的資料存取層 (DAL) 會使用具類型資料集。 中所述[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，具類型資料集包含強型別 Datatable 和 Tableadapter。 Datatable 系統時要執行資料存取工作的基礎資料庫的 Tableadapter 介面中代表邏輯實體。 這包括填入 Datatable 資料、 執行查詢會傳回純量的資料，並插入、 更新，和從資料庫刪除資料錄。

Tableadapter 所執行的 SQL 命令可以是任一個特定 SQL 陳述式，例如`SELECT columnList FROM TableName`，或預存程序。 在我們架構 Tableadapter 使用特定 SQL 陳述式。 許多開發人員和資料庫管理員，不過，而不用預存程序的安全性、 可維護性和可更新性理由的臨機操作 SQL 陳述式。 其他人 ardently 偏好使用彈性的特定 SQL 陳述式。 在我的工作中我勝過臨機操作 SQL 陳述式、 預存程序，但是選擇使用特定 SQL 陳述式來簡化較早的教學課程。

當定義 TableAdapter，或加入新的方法，TableAdapter 的精靈可讓就像您輕鬆地建立新的預存程序，或使用現有的預存程序，使用特定 SQL 陳述式一樣。 在本教學課程中，我們將檢驗如何讓 TableAdapter 的精靈自動產生預存程序。 在下一個教學課程中我們將探討如何設定 TableAdapter 的方法來使用現有的或以手動方式建立的預存程序。

> [!NOTE]
> 請參閱部落格文章： Rob Howard[不要使用預存程序尚未？](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/)和[Frans Bouma](https://weblogs.asp.net/fbouma/) s 部落格文章：[預存程序是不錯的想法，M Kay？](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx)的熱烈討論上的優缺點預存程序與特定 SQL。


## <a name="stored-procedure-basics"></a>預存程序的基本概念

函式是通用於所有的程式設計語言建構。 函式是函式呼叫時執行的陳述式的集合。 函式可以接受輸入的參數，而且可以選擇性地傳回值。 *[預存程序](http://en.wikipedia.org/wiki/Stored_procedure)* 資料庫建構函式的程式語言中具有共用許多相似之處。 預存程序是由一組預存程序呼叫時執行的 T-SQL 陳述式所組成。 預存程序可能會接受零到多個輸入參數，並可傳回純量值、 輸出參數，或者，大多數情況下，結果集`SELECT`查詢。

> [!NOTE]
> 預存程序通常稱為 sprocs 或預存程序。


使用建立預存程序[ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL 陳述式。 例如，下列 T-SQL 指令碼會建立名為預存程序`GetProductsByCategoryID`會採用單一參數，名為`@CategoryID`並傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`中資料行的欄位`Products`資料表具有相符的`CategoryID`值：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

一旦建立此預存程序之後，它可以使用下列語法來呼叫：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> 在下一個教學課程中，我們將檢查建立預存程序，透過 Visual Studio IDE。 此教學課程中，不過，我們要讓自動產生預存程序為我們的 TableAdapter 精靈。


除了只傳回資料，預存程序通常可用來執行多個資料庫命令，在單一交易範圍內。 名為預存程序`DeleteCategory`，比方說，可能需要`@CategoryID`參數，並執行兩個`DELETE`陳述式： 第一次，一刪除相關的產品，另一個刪除指定的類別目錄。 預存程序內的多個陳述式會*不*自動包裝在交易內。 其他的 T-SQL 命令需要發出以確保預存程序 s 多個命令會被視為不可部分完成的作業。 我們會看到如何將預存程序的命令包裝在後續的教學課程中的交易範圍內。

當使用預存程序的架構中，資料存取層的方法叫用特定的預存程序，而不是發出特定 SQL 陳述式。 這可以集中 （資料庫） 上執行的 SQL 陳述式的位置而不是讓應用程式的架構中定義它。 這個集中化說是可讓您更輕鬆地尋找、 分析及微調查詢，並提供清楚了解有關何處以及如何使用資料庫。

如需有關預存程序的基本概念的詳細資訊，請參閱本教學課程結尾處的進一步閱讀 > 一節中的資源。

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>步驟 1： 建立進階的資料存取層案例 Web 網頁

我們一開始我們討論建立 DAL，使用預存程序之前，可讓 s 先花點時間在我們的網站專案，我們需要此對話方塊和 下一步的幾個教學課程中建立 ASP.NET 網頁。 加入新的資料夾，名為啟動`AdvancedDAL`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![如需進階的資料存取層案例教學課程中加入 ASP.NET 網頁](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**圖 1**： 加入 ASP.NET 網頁的進階的資料存取層案例教學課程


如同在其他資料夾，`Default.aspx`中`AdvancedDAL`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


最後，將這些頁面加入做為項目至`Web.sitemap`檔案。 具體來說，批次的資料搭配使用之後加入下列標記`<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 左側功能表現在包含項目，如需進階的 DAL 案例教學課程。


![如需進階的 DAL 案例教學課程網站導覽現在包含的項目](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**圖 3**： 網站導覽現在包含項目，如需進階的 DAL 案例教學課程


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>步驟 2： 設定來建立新的 TableAdapter 預存程序

若要示範建立資料存取層，而不是特定 SQL 陳述式會使用預存程序，讓 s 建立新的輸入資料集在`~/App_Code/DAL`資料夾名為`NorthwindWithSprocs.xsd`。 因為我們已在先前的教學課程中逐步執行此程序的詳細資料，我們會繼續執行快速透過這裡的步驟。 如果您碰到需要進一步建立及設定類型的 DataSet 中的逐步指示，請參閱上一步[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程。

將新的資料集加入至專案中，以滑鼠右鍵按一下`DAL`資料夾中，選擇 加入新項目和選取的資料集的範本，如圖 4 所示。


[![將新的具類型資料集加入至名為 NorthwindWithSprocs.xsd 的專案](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**圖 4**： 將新的輸入資料集加入至專案名為`NorthwindWithSprocs.xsd`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


這會建立新的輸入資料集、 開啟其設計工具、 建立新的 TableAdapter，並啟動 [TableAdapter 組態精靈]。 TableAdapter 組態精靈 s 第一個步驟會詢問我們選取要使用的資料庫。 Northwind 資料庫的連接字串應該會列出下拉式清單中。 選取此選項，然後按一下 [下一步]。

從這個下一個畫面中，我們可以選擇 TableAdapter 應如何存取資料庫。 在上一個教學課程中，我們會選取第一個選項，則使用 SQL 陳述式。 此教學課程中，選取第二個選項，建立新的預存程序，然後按一下下一步。


[![指示建立新的預存程序 TableAdpater](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**圖 5**： 指示來建立新的預存程序 TableAdpater ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


就像使用特定 SQL 陳述式下, 一個步驟中我們會要求您提供`SELECT`TableAdapter s 主查詢陳述式。 但不使用`SELECT`此處輸入直接進行臨機操作查詢陳述式，TableAdapter 的精靈會建立包含此預存程序`SELECT`查詢。

使用下列`SELECT`此 TableAdapter 的查詢：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![輸入選取的查詢](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**圖 6**： 輸入`SELECT`查詢 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> 上述查詢會稍有不同的主查詢`ProductsTableAdapter`中`Northwind`型別資料集。 請記得，`ProductsTableAdapter`中`Northwind`具類型資料集包含兩個相互關聯子查詢，以回復分類名稱和每個 product s category] 和 [供應商的公司名稱。 在即將[更新為使用聯結的 TableAdapter](updating-the-tableadapter-to-use-joins-vb.md)教學課程中我們將探討指令碼加入至這個 TableAdapter 相關資料。


請花一點時間按一下 [進階選項] 按鈕。 從這裡，我們可以指定是否精靈應該也要產生 insert、 update 和 delete 陳述式的 TableAdapter，是否使用開放式並行存取，以及是否資料應該可重新整理資料表之後插入和更新。 預設為核取 產生 Insert、 Update 和 Delete 陳述式選項。 保留檢查。 本教學課程，請不要使用開放式並行存取選項選取。

TableAdapter 精靈會自動建立的預存程序時，它會出現重新整理資料的資料表選項會被忽略。 不論是否選取這個核取方塊，產生的 insert 和 update 預存程序會擷取在步驟 3 中，我們會看到剛插入，或只更新的記錄。


![保留的核取選項產生 Insert、 Update 和 Delete 陳述式](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**圖 7**： 保留的核取選項產生 Insert、 Update 和 Delete 陳述式


> [!NOTE]
> 如果已核取 使用開放式並行存取選項，精靈會將額外的條件`WHERE`避免如果沒有變更其他欄位中更新資料的子句。 若要回頭參考[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)教學課程，如需有關使用 TableAdapter s 內建的開放式並行存取控制功能。


輸入後`SELECT`查詢，並確認已核取 產生 Insert、 Update 和 Delete 陳述式選項，按一下 下一步。 接下來的畫面，圖 8 所示提示的選取、 插入、 更新和刪除資料精靈將建立的預存程序名稱。 這些預存程序名稱，以變更`Products_Select`， `Products_Insert`， `Products_Update`，和`Products_Delete`。


[![重新命名預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**圖 8**： 重新命名預存程序 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


若要查看 T-SQL TableAdapter 精靈將使用來建立四個預存程序，請按一下 [預覽 SQL 指令碼] 按鈕。 從預覽 SQL 指令碼 對話方塊中您可能將指令碼儲存至檔案，或將它複製到剪貼簿。


![預覽用來產生預存程序的 SQL 指令碼](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**圖 9**： 預覽 SQL 指令碼用來產生預存程序


命名預存程序之後, 按一下旁邊的 tableadapter 名稱對應的方法。 如同時使用特定 SQL 陳述式，我們可以建立會填入現有的資料表或傳回一個新方法。 我們也可以指定 TableAdapter 是否應包含的 DB Direct 模式，插入、 更新和刪除的記錄。 保留所有的三個核取方塊已核取，但重新命名傳回 DataTable 方法`GetProducts`（如圖 10 所示）。


[![命名方法填滿和 GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**圖 10**: Name 方法`Fill`和`GetProducts`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


按一下 [下一步] 若要查看精靈將要執行之步驟的摘要。 完成精靈，依序按一下 [完成] 按鈕。 精靈完成後，您會返回 dataset 設計工具，現在應該包含`ProductsDataTable`。


[![資料集 s 設計工具會顯示新加入的 ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**圖 11**: dataset 設計工具會顯示新加入`ProductsDataTable`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>步驟 3： 檢查新建立的預存程序

[Tabaleadapter 精靈] 會自動使用在步驟 2 中建立選取、 插入、 更新和刪除資料的預存程序。 這些預存程序可檢視或修改透過 Visual Studio 移至 [伺服器總管] 中，並向下切入資料庫 s 預存程序資料夾。 圖 12 示，Northwind 資料庫包含四個新的預存程序： `Products_Delete`， `Products_Insert`， `Products_Select`，和`Products_Update`。


![步驟 2 中建立的四個預存程序可以找到資料庫 s 預存程序資料夾中](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**圖 12**： 步驟 2 中建立的四個預存程序可以找到資料庫 s 預存程序資料夾中


> [!NOTE]
> 如果看不到 [伺服器總管] 中，請移至 [檢視] 功能表，然後選擇 [伺服器總管] 選項。 如果看不到產品相關預存程序新增從步驟 2，請再試一次預存程序資料夾上按一下滑鼠右鍵，然後選擇重新整理。


檢視或修改預存程序，請按兩下其名稱，在 伺服器總管 或，或者，以滑鼠右鍵按一下預存程序，並選擇 開啟。 圖 13`Products_Delete`預存程序中，開啟時。


[![您可以開啟並從 Visual Studio 中修改預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**圖 13**： 預存程序可以開啟和修改從內 Visual Studio ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


兩者的內容`Products_Delete`和`Products_Select`預存程序會很簡單。 `Products_Insert`和`Products_Update`預存程序，相反地，需要仔細的檢查與這兩個執行`SELECT`之後的陳述式其`INSERT`和`UPDATE`陳述式。 例如，下列 SQL 組成`Products_Insert`預存程序：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

做為輸入參數的預存程序接受`Products`所傳回的資料行`SELECT`TableAdapter 的精靈和這些值，指定查詢中使用`INSERT`陳述式。 遵循`INSERT`陳述式，`SELECT`查詢用來傳回`Products`資料行值 (包括`ProductID`) 的新加入的記錄。 此重新整理功能時，加入新的記錄，因為它使用批次更新模式時，也將會自動更新新加入`ProductRow`執行個體`ProductID`屬性指派資料庫的自動遞增的值。

下列程式碼說明這項功能。 它包含`ProductsTableAdapter`和`ProductsDataTable`建立`NorthwindWithSprocs`型別資料集。 藉由建立新的產品加入至資料庫`ProductsRow`執行個體，提供其值，然後呼叫 tableadapter`Update`方法，傳入`ProductsDataTable`。 就內部而言，tableadapter`Update`方法會列舉`ProductsRow`傳入的 DataTable 中的執行個體 （在這個範例有一個只-我們剛才加入的一個），並執行適當的插入、 更新或刪除命令。 在此情況下，`Products_Insert`執行預存程序時，這樣會將新的記錄，到`Products`資料表，並傳回新加入的記錄的詳細資訊。 `ProductsRow`執行個體的`ProductID`然後更新值。 之後`Update`方法已完成，所以我們可以存取的新加入的資料錄 s`ProductID`值透過`ProductsRow`s`ProductID`屬性。


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

`Products_Update`同樣包含預存程序`SELECT`之後的陳述式其`UPDATE`陳述式。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

請注意，這個預存程序包括兩個輸入的參數的`ProductID`:`@Original_ProductID`和`@ProductID`。 可能變更主索引鍵的情況下允許這項功能。 比方說，在員工資料庫中，每位員工的記錄可能會使用員工的社會安全號碼做為其主索引鍵。 若要變更現有員工 s 身分證號碼，就必須提供新的身分證號碼和原始。 如`Products`不需要這類功能的資料表，因為`ProductID`資料行是`IDENTITY`資料行，而且永遠不會變更。 事實上，`UPDATE`陳述式中的`Products_Update`預存程序不包含`ProductID`其資料行清單中的資料行。 因此，雖然`@Original_ProductID`用於`UPDATE`陳述式 s`WHERE`子句，它是多餘的`Products`資料表，並取代`@ProductID`參數。 修改預存程序的參數時請務必也會更新該預存程序的 TableAdapter 方法。

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>步驟 4： 修改預存程序的參數，並更新 TableAdapter

因為`@Original_ProductID`參數為多餘，可讓 s 移除從`Products_Update`完全預存程序。 開啟`Products_Update`預存程序，刪除`@Original_ProductID`參數，然後在`WHERE`子句`UPDATE`陳述式中，變更參數名稱會用於從`@Original_ProductID`至`@ProductID`。 進行這些變更之後，預存程序內的 T-SQL 看起來應該如下所示：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

若要將這些變更儲存至資料庫中，按一下工具列中的 [儲存] 圖示或按 Ctrl + S。 此時，`Products_Update`預存程序不會預期`@Original_ProductID`輸入的參數，但 TableAdapter 設定為傳遞這種參數。 您可以看到的 TableAdapter 會將傳送到參數`Products_Update`選取 TableAdapter DataSet 設計工具中，移至 [屬性] 視窗中，然後按一下省略符號，預存程序`UpdateCommand`s`Parameters`集合。 這會顯示圖 14 在參數集合編輯器對話方塊。


![參數集合編輯器清單所使用的參數傳遞至 Products_Update 預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**圖 14**： 參數集合編輯器清單所使用的參數傳遞至`Products_Update`預存程序


您可以從這裡移除此參數，只要選取`@Original_ProductID`參數從清單中的成員，然後按一下 [移除] 按鈕。

或者，您可以重新整理設計工具的 TableAdapter 上按一下滑鼠右鍵，然後選擇 設定所使用的所有方法的參數。 這會顯示 TableAdapter 組態精靈，列出用來選取、 插入、 更新預存程序，並刪除，以及參數的預存程序應該會收到。 如果您在更新下拉式清單按一下您所見`Products_Update`預存程序必須是輸入的參數，現在不再包括`@Original_ProductID`（請參閱圖 15）。 只要按一下 [完成]，自動更新 TableAdapter 所使用的參數集合。


[![或者，您可以使用 [TableAdapter 的組態精靈] 以重新整理其方法的參數集合](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**圖 15**： 您也可以使用 [tableadapter 組態精靈] 重新整理它的方法參數集合 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>步驟 5： 加入其他的 TableAdapter 方法

如同步驟 2 中所述，建立新的 TableAdapter 時很容易讓有對應的預存程序，以自動產生。 加入 TableAdapter 的其他方法時，也是如此。 為了說明這點，可讓 s 新增`GetProductByProductID(productID)`方法`ProductsTableAdapter`步驟 2 中建立。 這個方法會將做為輸入`ProductID`值，並傳回指定之產品的相關詳細資料。

啟動 TableAdapter 上按一下滑鼠右鍵，然後從內容功能表中選擇 加入查詢。


![將新的查詢加入至 TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**圖 16**： 將新的查詢加入至 TableAdapter


這會啟動 TableAdapter 查詢組態精靈，它第一次會提示輸入 TableAdapter 應如何存取資料庫。 若要建立的新預存程序，請選擇 建立新的預存程序選項，並按一下 下一步。


[![選擇 建立新的預存程序選項](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**圖 17**： 選擇 建立新的預存程序選項 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


下一個畫面會詢問我們識別來執行時，是否將會傳回一組資料列或單一純量值，或執行查詢的型別`UPDATE`， `INSERT`，或`DELETE`陳述式。 因為`GetProductByProductID(productID)`方法會傳回一個資料列，保留選取會傳回選取的資料列的選項，然後按 [下一步]。


[![選擇 選取會傳回資料列選項](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**圖 18**： 選擇 選取會傳回資料列選項 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


下一個畫面會顯示 TableAdapter s 主查詢，因為它只會列出預存程序的名稱 (`dbo.Products_Select`)。 取代為下列預存程序名稱`SELECT`陳述式，會傳回所有指定的產品的產品欄位：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![使用 SELECT 查詢來取代預存程序名稱](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**圖 19**： 取代預存程序名稱與`SELECT`查詢 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


後續的畫面會要求您將建立的預存程序的名稱。 輸入名稱`Products_SelectByProductID`，按一下 [下一步]。


[![新的預存程序 Products_SelectByProductID 名稱](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**圖 20**： 命名新的預存程序`Products_SelectByProductID`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


精靈的最後一個步驟可讓我們來變更的方法名稱產生，以及指出是否要使用的填滿 DataTable 模式、 傳回 DataTable 模式中，或兩者。 這個方法中，保留選取此選項，這兩個選項，但是重新命名的方法`FillByProductID`和`GetProductByProductID`。 按一下 [下一步] 若要檢視精靈會執行，並接著按一下 [完成] 以完成精靈步驟的摘要。


[![重新命名為 FillByProductID 和 GetProductByProductID 的 TableAdapter 的方法](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**圖 21**： 重新命名的 TableAdapter 的方法`FillByProductID`和`GetProductByProductID`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


完成精靈之後，TableAdapter 有新的方法， `GetProductByProductID(productID)` ，叫用時，會執行`Products_SelectByProductID`預存程序已建立。 請花一點時間鑽研預存程序 」 資料夾，並開啟檢視這個新的預存程序從 伺服器總管`Products_SelectByProductID`（如果您有看到，預存程序資料夾上按一下滑鼠右鍵並選擇 重新整理）。

請注意，`SelectByProductID`預存程序會採用`@ProductID`做為輸入參數，並執行`SELECT`我們在精靈中輸入陳述式。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>步驟 6： 建立商務邏輯層類別

在整個教學課程系列我們有 strived 維護展示層進行的所有商務邏輯層 (BLL) 其呼叫的多層式的架構。 若要遵循此設計決策，我們需要 BLL 類別建立新的輸入資料集，我們可以從展示層存取產品資料之前。

建立新的類別檔案命名為`ProductsBLLWithSprocs.vb`中`~/App_Code/BLL`資料夾並加入下列程式碼：


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

這個類別會模擬`ProductsBLL`類別從較早的教學課程，但會使用語意`ProductsTableAdapter`和`ProductsDataTable`物件從`NorthwindWithSprocs`資料集。 例如，而不是讓`Imports NorthwindTableAdapters`類別檔案，做為開頭的陳述式`ProductsBLL`，`ProductsBLLWithSprocs`類別會使用`Imports NorthwindWithSprocsTableAdapters`。 同樣地，`ProductsDataTable`和`ProductsRow`用於這個類別中的物件會加上`NorthwindWithSprocs`命名空間。 `ProductsBLLWithSprocs`類別提供兩個資料存取方法，`GetProducts`和`GetProductByProductID`，和方法，以新增、 更新和刪除的單一產品的執行個體。

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>步驟 7： 使用`NorthwindWithSprocs`從展示層的資料集

現在我們已經建立 DAL 使用預存程序來存取和修改基礎資料庫資料。 我們也已經建立初步 BLL 與方法，以擷取所有的產品或特定產品以及方法來加入、 更新及刪除產品。 要捨入關閉本教學課程，讓 s 建立 ASP.NET 網頁使用 BLL 的`ProductsBLLWithSprocs`類別顯示、 更新和刪除資料錄。

開啟`NewSprocs.aspx`頁面`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具，其命名為拖曳 GridView `Products`。 從 GridView s 智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定用於 ObjectDataSource`ProductsBLLWithSprocs`類別，如圖 22 中所示。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**圖 22**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


下拉式清單選取索引標籤中的有兩個選項，`GetProducts`和`GetProductByProductID`。 由於我們想在 GridView 中顯示所有產品，請選擇`GetProducts`方法。 UPDATE、 INSERT 和 DELETE 索引標籤中每個下拉式清單中只能有一個方法。 請確認這些下拉式清單具有其選取適當的方法，然後按一下 [完成] 5d;。

在 ObjectDataSource 精靈完成之後，Visual Studio 將會加入 BoundFields 及其產品資料欄位的 GridView。 檢查啟用編輯和中的智慧標籤的 啟用刪除選項開啟 GridView s 內建編輯和刪除功能。


[![此頁面包含編輯和刪除啟用支援的 GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**圖 23**： 此頁面包含編輯和刪除啟用支援的 GridView ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


當我們已討論在上一個教學課程中，在精靈完成時 ObjectDataSource s，Visual Studio 設定`OldValuesParameterFormatString`屬性至原始\_{0}。 這需要還原為其預設值，為了讓資料修改功能運作 {0} 的正確指定我們 BLL 中的方法所預期的參數。 因此，請務必設定`OldValuesParameterFormatString`{0} 屬性或屬性完全移除的宣告式語法。

完成設定資料來源精靈中，開啟 編輯和刪除在 GridView 中支援和傳回 ObjectDataSource s 之後`OldValuesParameterFormatString`屬性為其預設值，您的網頁 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

此時我們無法不妨整理一下 GridView 藉由自訂編輯介面，以包含驗證，具有`CategoryID`和`SupplierID`資料行轉譯為 DropDownLists，依此類推。 我們也可以新增用戶端確認至 [刪除] 按鈕，並建議您花一些時間來實作這些增強功能。 因為先前的教學課程涵蓋這些主題，不過，我們將不涵蓋這些項目一次這裡。

不論是否增強 GridView 與否，測試頁核心功能，在瀏覽器中。 如圖 24 所示，頁面會列出提供每個資料列編輯和刪除功能的 GridView 中的產品。


[![產品可以檢視、 編輯和刪除從 GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**圖 24**: 產品可以檢視、 編輯，而刪除從 GridView ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>總結

Tableadapter 中具類型資料集可以存取資料，使用特定 SQL 陳述式從資料庫或透過預存程序。 當使用預存程序，其中一個現有的預存程序可以使用，或可以指示 [Tabaleadapter 精靈] 建立新的預存程序根據`SELECT`查詢。 在本教學課程中，我們探索如何為我們自動建立的預存程序。

在具有自動產生有助於節省時間的預存程序，時有特定精靈規定 t 所建立的預存程序配合什麼我們會建立自己的情況。 其中一個範例是`Products_Update`預存程序，必須同時是`@Original_ProductID`和`@ProductID`輸入參數，即使`@Original_ProductID`參數為非必要。

在許多情況下，預存程序可能已經建立，或是我們想要以手動方式，以便有更好的控制預存程序的命令來建置。 在任一情況下，我們會想要使用現有的預存程序為其方法 TableAdapter 的指示。 我們應該了解如何完成這項作業在下一個教學課程。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建立和維護預存程序](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [從預存程序擷取純量的資料](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server 預存程序的基本概念](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [預存程序： 概觀](http://www.sqlteam.com/item.asp?ItemID=563)
- [撰寫預存程序](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Geisenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [下一頁](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
