---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: 正在建立新的預存程序為具類型資料集的 Tableadapter (VB) |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中我們有我們的程式碼中建立 SQL 陳述式，並傳遞到資料庫來執行的陳述式。 另一個方法是使用 s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: bc640564cfb67f0c1512bc7f4fae9ea7e6bc981f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833860"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>正在建立新的預存程序為具類型資料集的 Tableadapter (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip)或[下載 PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> 在先前的教學課程中我們有我們的程式碼中建立 SQL 陳述式，並傳遞到資料庫來執行的陳述式。 另一個方法是使用預存程序，其中的 SQL 陳述式是預先定義的資料庫。 在本教學課程中，我們了解如何讓 TableAdapter 精靈為我們產生新的預存程序的內容。


## <a name="introduction"></a>簡介

這些教學課程中的資料存取層 (DAL) 會使用具類型資料集。 中所述[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，具類型資料集包含強型別 DataTables 和 Tableadapter。 Datatable 代表基礎資料庫以執行資料存取公司的 TableAdapters 介面時，系統的邏輯實體。 這包括填入 Datatable 的資料、 執行傳回純量資料的查詢和插入、 更新和從資料庫刪除資料錄。

Tableadapter 所執行的 SQL 命令可以是任一特定 SQL 陳述式中，例如`SELECT columnList FROM TableName`，或預存程序。 在我們的架構 Tableadapter 會使用特定 SQL 陳述式。 許多開發人員和資料庫管理員，不過，而不用預存程序安全性、 可維護性，以及可更新性原因的特定 SQL 陳述式。 其他人 ardently 偏好其彈性的特定 SQL 陳述式。 我自己的工作中，我讓預存程序優先於特定 SQL 陳述式，但選擇要使用特定 SQL 陳述式來簡化先前的教學課程。

當定義 TableAdapter，或加入新的方法，TableAdapter 的精靈可就如同您輕鬆地建立新的預存程序，或使用現有的預存程序，其方式就如同使用特定 SQL 陳述式。 在本教學課程中，我們將檢驗如何讓 TableAdapter 的精靈將自動產生預存程序。 在下一個教學課程中我們將探討如何設定 TableAdapter 的方法來使用現有的或以手動方式建立的預存程序。

> [!NOTE]
> 請參閱 Rob Howard 的部落格項目[不要使用預存程序尚未？](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/)和[別 Bouma](https://weblogs.asp.net/fbouma/) s 部落格項目[預存程序是不錯的想法，M Kay？](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx)的熱烈爭論的優缺點預存程序和特定 SQL。


## <a name="stored-procedure-basics"></a>預存程序的基本概念

函式是通用於所有的程式設計語言建構。 函式是函式呼叫時執行的陳述式的集合。 函式可以接受輸入的參數，而且可以選擇性地傳回值。 *[預存程序](http://en.wikipedia.org/wiki/Stored_procedure)* 是資料庫的建構函式程式設計語言中的共用許多相似之處。 預存程序是組成一組呼叫預存程序時執行 T-SQL 陳述式。 預存程序可能會接受零到多輸入參數，並可傳回純量值、 輸出參數，或者，從結果集的大多數情況下，`SELECT`查詢。

> [!NOTE]
> 預存程序通常稱為 sprocs 或預存程序。


使用建立預存程序[ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL 陳述式。 例如，下列 T-SQL 指令碼會建立名為預存程序`GetProductsByCategoryID`會採用具有單一參數具名`@CategoryID`，並傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`中資料行的欄位`Products`資料表中具有相符`CategoryID`值：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

一旦建立此預存程序之後，它可以呼叫使用下列語法：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> 在下一個教學課程中，我們將檢查建立預存程序，透過 Visual Studio IDE。 本教學課程中，不過，我們要讓 [Tabaleadapter 精靈] 會自動為我們產生的預存程序。


除了只傳回資料，預存程序通常用於執行單一交易範圍內的多個資料庫命令。 名為預存程序`DeleteCategory`，比方說，可能需要`@CategoryID`參數，並執行兩個`DELETE`陳述式： 第一次，一刪除相關的產品，而另一個刪除指定的分類。 預存程序內的多個陳述式*不*自動包裝在交易內。 其他的 T-SQL 命令需要發出以確保預存程序 s，多個命令會被視為不可部分完成的作業。 我們會看到如何將預存程序的命令包裝在後續的教學課程中的交易範圍內。

使用預存程序的架構中，資料存取層的方法叫用特定的預存程序，而不是發出特定 SQL 陳述式。 這樣可以集中 （在資料庫中） 上執行的 SQL 陳述式的位置而不是讓它在應用程式的架構定義。 這個集中化說是可讓您更輕鬆地尋找、 分析和微調查詢，並提供清楚了解有關位置和方式所使用的資料庫。

如需有關預存程序的基本概念的詳細資訊，請參閱在本教學課程結尾處的進一步閱讀 > 一節中的資源。

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>步驟 1： 建立進階的資料存取層案例網頁

我們開始討論有關建立 DAL，使用預存程序之前，讓先花點時間在網站專案中，我們需要此對話方塊和下一步 的數個教學課程中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`AdvancedDAL`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![加入 ASP.NET 網頁的進階的資料存取層的案例教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**圖 1**： 加入 ASP.NET 網頁的進階的資料存取層的案例教學課程


在其他資料夾，例如`Default.aspx`在`AdvancedDAL`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


最後，將這些頁面新增項目為`Web.sitemap`檔案。 具體來說，處理批次的資料之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含項目，為進階的 DAL 案例教學課程。


![網站導覽現在包含項目為進階的 DAL 案例教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**圖 3**： 網站地圖現在包含項目為進階的 DAL 案例教學課程


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>步驟 2： 設定 TableAdapter 建立新的預存程序

為了示範如何建立資料存取層，而不是特定 SQL 陳述式會使用預存程序，可讓 s 會建立新的具類型資料集在`~/App_Code/DAL`名為資料夾`NorthwindWithSprocs.xsd`。 因為我們已逐步執行此程序的詳細資料，在先前的教學課程中，我們將繼續快速完成的步驟。 如果您卡，或需要進一步中建立及設定輸入資料集的逐步指示，請參閱上一步[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程。

將新的資料集加入至專案中，以滑鼠右鍵按一下`DAL`資料夾中，選擇 加入新項目，然後選取 資料集 範本，如 圖 4 所示。


[![將新的具類型資料集加入至名為 NorthwindWithSprocs.xsd 專案](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**圖 4**： 將新的具類型資料集加入至專案命名為`NorthwindWithSprocs.xsd`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


這會建立新的具類型資料集、 開啟它的設計工具、 建立新的 TableAdapter，並啟動 [TableAdapter 組態精靈]。 TableAdapter 組態精靈 s 第一個步驟會要求我們選取要使用的資料庫。 Northwind 資料庫的連接字串應該列在下拉式清單中。 選取此選項，然後按一下 [下一步]。

這個下一步 畫面中，我們可以選擇 TableAdapter 應如何存取資料庫。 在上一個教學課程中，我們會選取第一個選項，使用 SQL 陳述式。 本教學課程中，選取第二個選項，建立新的預存程序，並按一下 [下一步]。


[![指示來建立新的預存程序 TableAdpater](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**圖 5**： 指示來建立新的預存程序 TableAdpater ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


就像使用特定 SQL 陳述式，在下一個步驟中我們會要求您提供`SELECT`TableAdapter s 主查詢的陳述式。 但為了取代使用`SELECT`若要執行臨機操作查詢，直接在此處輸入的陳述式，TableAdapter 的精靈會建立包含此預存程序`SELECT`查詢。

使用下列項目`SELECT`這個 TableAdapter 的查詢：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![輸入 SELECT 查詢](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**圖 6**： 輸入`SELECT`查詢 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> 上述查詢會稍有不同的主查詢`ProductsTableAdapter`在`Northwind`具類型資料集。 請記得，`ProductsTableAdapter`在`Northwind`具類型資料集包含兩個相互關聯子查詢，以重新顯示每個 product s category] 和 [供應商的公司名稱與類別名稱。 即將上市[更新 TableAdapter 以使用聯結](updating-the-tableadapter-to-use-joins-vb.md)教學課程中我們將探討將以下內容新增至這個 TableAdapter 相關資料。


花點時間按一下 [進階選項] 按鈕。 我們可以從這裡，指定是否精靈應該也產生 insert、 update 和 delete 陳述式的 TableAdapter、 是否要使用開放式並行存取，以及是否資料表應該重新整理之後插入和更新。 依預設會勾選 產生 Insert、 Update 和 Delete 陳述式選項。 保留選取它。 本教學課程中，請勿使用開放式並行存取選項核取。

TableAdapter 精靈會自動建立的預存程序時，它會顯示重新整理資料的資料表選項會被忽略。 不論是否已核取此核取方塊，產生的 insert 和 update 預存程序會擷取在步驟 3 中，我們會看到剛才插入，或只是更新的記錄。


![保留核取選項產生 Insert、 Update 和 Delete 陳述式](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**圖 7**： 保留核取選項產生 Insert、 Update 和 Delete 陳述式


> [!NOTE]
> 如果已使用開放式並行存取選項，精靈會將額外的條件`WHERE`防止如果沒有變更其他欄位中更新資料的子句。 回頭[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)教學課程，如需使用 TableAdapter s 內建的開放式並行存取控制功能的詳細資訊。


輸入後`SELECT`查詢，並確認已核取 產生 Insert、 Update 和 Delete 陳述式選項中，按一下 下一步。 接下來的畫面，顯示在 圖 8 中，會提示選取、 插入、 更新和刪除資料的精靈將建立的預存程序名稱。 這些預存程序名稱，來變更`Products_Select`， `Products_Insert`， `Products_Update`，和`Products_Delete`。


[![重新命名預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**圖 8**： 重新命名預存程序 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


若要查看 T-SQL TableAdapter 精靈將會使用建立四個預存程序，請按一下 [預覽 SQL 指令碼] 按鈕。 從預覽 SQL 指令碼 對話方塊中，您可能將指令碼儲存至檔案或將它複製到剪貼簿。


![預覽 SQL 指令碼，用來產生預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**圖 9**： 預覽 SQL 指令碼用來產生預存程序


命名預存程序之後, 按一下旁邊的 tableadapter 名稱對應的方法。 就像時使用特定 SQL 陳述式，我們可以建立會填入現有的資料表或傳回一個新的方法。 我們也可以指定 TableAdapter 是否應包含的 DB 直接模式，插入、 更新和刪除的記錄。 保留所有的三個核取方塊已核取，但重新命名傳回的 DataTable 方法`GetProducts`（如 [圖 10] 所示）。


[![命名方法填滿 與 GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**圖 10**: Name 方法`Fill`並`GetProducts`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


按一下 [下一步] 以查看精靈即將執行之步驟的摘要。 按一下 [完成] 按鈕，以完成精靈。 精靈完成後，您將返回 dataset 設計工具，現在應該包含`ProductsDataTable`。


[![資料集 s 設計工具會顯示新加入的 ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**圖 11**: The dataset 設計工具會顯示新加入`ProductsDataTable`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>步驟 3： 檢查新建立的預存程序

[Tabaleadapter 精靈] 會自動使用在步驟 2 中建立選取、 插入、 更新和刪除資料的預存程序。 這些預存程序可檢視或修改透過 Visual Studio，移至 [伺服器總管] 中，並向下切入至資料庫預存程序資料夾。 Northwind 資料庫如 [圖 12] 所示，包含四個新的預存程序： `Products_Delete`， `Products_Insert`， `Products_Select`，和`Products_Update`。


![步驟 2 中建立四個預存程序可在資料庫 s 預存程序 資料夾](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**[圖 12**： 步驟 2 中建立四個預存程序可在資料庫 s 預存程序] 資料夾


> [!NOTE]
> 如果看不到 [伺服器總管] 中，請移至 [檢視] 功能表，然後選擇 [伺服器總管] 選項。 如果看不到產品相關預存程序步驟 2 中新增，請嘗試 [預存程序] 資料夾上按一下滑鼠右鍵並選擇將重新整理。


檢視或修改預存程序，請按兩下其名稱，在 [伺服器總管] 或，或者，預存程序上按一下滑鼠右鍵並選擇 [開啟]。 [圖 13] 顯示`Products_Delete`開啟時，預存程序。


[![您可以開啟並從 Visual Studio 中修改預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**圖 13**： 預存程序可以開啟和修改從在 Visual Studio ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


這兩者的內容`Products_Delete`和`Products_Select`預存程序都相當直接明瞭。 `Products_Insert`並`Products_Update`預存程序，相反地，需要仔細檢查這兩個執行`SELECT`之後的陳述式及其`INSERT`和`UPDATE`陳述式。 例如，下列 SQL 組成`Products_Insert`預存程序：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

做為輸入參數的預存程序接受`Products`所傳回的資料行`SELECT`TableAdapter 的精靈和這些值中指定的查詢中使用`INSERT`陳述式。 遵循`INSERT`陳述式中，`SELECT`查詢用來傳回`Products`資料行值 (包括`ProductID`) 的新加入的記錄。 此重新整理功能很實用的當加入新的記錄，因為它使用批次更新模式時，也將會自動更新新增`ProductRow`執行個體`ProductID`自動遞增的值指派資料庫所使用的屬性。

下列程式碼會示範這項功能。 它包含`ProductsTableAdapter`並`ProductsDataTable`針對建立`NorthwindWithSprocs`具類型資料集。 將新產品時，會加入至資料庫上，藉由建立`ProductsRow`執行個體，提供其值，並呼叫 tableadapter`Update`方法並傳入`ProductsDataTable`。 就內部而言，tableadapter`Update`方法會列舉`ProductsRow`傳入的 DataTable 中的執行個體 （在此範例中有一個只-我們只是新增），並執行適當的插入、 更新或刪除命令。 在此情況下，`Products_Insert`執行預存程序時，它會將新增到新的記錄`Products`資料表，並傳回新加入的記錄詳細資料。 `ProductsRow`執行個體的`ProductID`再更新值。 在後`Update`方法完成，我們也可以存取的新加入的資料錄 s`ProductID`透過值`ProductsRow`s`ProductID`屬性。


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

`Products_Update`同樣地包含預存程序`SELECT`之後的陳述式其`UPDATE`陳述式。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

請注意，這個預存程序包含兩個輸入的參數`ProductID`:`@Original_ProductID`和`@ProductID`。 這項功能可讓主索引鍵可能會變更的案例。 例如，在員工資料庫，每位員工的記錄可能會使用員工的社會安全號碼作為其主索引鍵。 若要變更現有員工的社會安全號碼，就必須提供新的社會安全號碼和原始。 針對`Products`資料表，這類功能不需要因為`ProductID`資料行是`IDENTITY`資料行，並應該永遠不會變更。 事實上，`UPDATE`中的陳述式`Products_Update`預存程序不包含`ProductID`其資料行清單中的資料行。 因此，雖然`@Original_ProductID`用於`UPDATE`陳述式 s`WHERE`子句，它是多餘的`Products`資料表，並可以取代`@ProductID`參數。 修改預存程序的參數時，這是很重要，也會更新使用該預存程序的 TableAdapter 方法。

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>步驟 4： 修改預存程序的參數，並更新 TableAdapter

由於`@Original_ProductID`參數為非必要，可讓 s 中移除從`Products_Update`完全預存程序。 開啟`Products_Update`預存程序，刪除`@Original_ProductID`參數，然後在`WHERE`子句`UPDATE`陳述式中，變更參數名稱會用於從`@Original_ProductID`至`@ProductID`。 進行這些變更之後，在預存程序中的 T-SQL 看起來應該如下所示：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

若要將這些變更儲存至資料庫中，按一下工具列中的 [儲存] 圖示或按 Ctrl + S。 在此時`Products_Update`預存程序不會預期`@Original_ProductID`輸入的參數，但 TableAdapter 已將這類參數傳遞。 您可以看到 TableAdapter 會將傳送至的參數`Products_Update`預存程序選取 DataSet 設計工具中的 TableAdapter、 移至 屬性 視窗中，並按一下中的省略符號`UpdateCommand`s`Parameters`集合。 此時會出現 圖 14 所示的參數集合編輯器 對話方塊。


![參數集合編輯器清單所使用的參數傳遞至 Products_Update 預存程序](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**圖 14**： 參數集合編輯器清單所使用的參數傳遞至`Products_Update`預存程序


您可以從這裡移除此參數，只要選取`@Original_ProductID`參數從清單中的成員，然後按一下 [移除] 按鈕。

或者，您可以重新整理的所有方法在設計工具中的 TableAdapter 上按一下滑鼠右鍵，然後選擇 設定所使用的參數。 這會顯示 [TableAdapter 組態精靈]，列出用於選取、 插入、 更新預存程序，並刪除，以及參數預期接收的預存程序。 如果您按一下 更新 下拉式清單上您所見`Products_Update`預存程序會預期輸入的參數，現在不會再包含`@Original_ProductID`（請參閱 圖 15）。 只要按一下 完成，自動更新 TableAdapter 所使用的參數集合。


[![您也可以使用 [TableAdapter 的組態精靈]，重新整理其方法的參數集合](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**圖 15**： 您也可以使用 [tableadapter 組態精靈] 重新整理其方法的參數集合 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>步驟 5： 新增額外的 TableAdapter 方法

步驟 2 中所述，為建立新的 TableAdapter 時很容易有對應的預存程序，以自動產生。 加入 TableAdapter 的其他方法時，相同為 true。 為了說明這點，可讓新增的 s`GetProductByProductID(productID)`方法，以`ProductsTableAdapter`步驟 2 中建立。 這個方法會將做為輸入`ProductID`值，並傳回指定之產品的相關詳細資料。

啟動 TableAdapter 上按一下滑鼠右鍵，然後從內容功能表中選擇加入查詢。


![將新的查詢加入至 TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**圖 16**： 將新的查詢加入至 TableAdapter


這會啟動 TableAdapter 查詢組態精靈，它首先會提示輸入 TableAdapter 應如何存取資料庫。 若要建立的新預存程序，請選擇 建立新的預存程序選項，按一下 下一步。


[![選擇 建立新的預存程序選項](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**圖 17**： 選擇 建立新的預存程序選項 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


下一個畫面詢問我們找出要執行時，是否會傳回一組資料列或單一純量值，或執行查詢的型別`UPDATE`， `INSERT`，或`DELETE`陳述式。 因為`GetProductByProductID(productID)`方法會傳回一個資料列，請將保留選取會傳回資料列的選項選取，然後按 [下一步]。


[![選擇 選取會傳回資料列選項](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**圖 18**： 選擇 選取會傳回資料列選項 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


下一個畫面會顯示 TableAdapter s 主要查詢，因為它只會列出的預存程序名稱 (`dbo.Products_Select`)。 預存程序的名稱取代為下列`SELECT`陳述式，它會傳回所有指定的產品的產品欄位：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![將預存程序名稱取代為 SELECT 查詢](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**圖 19**： 將預存程序名稱取代`SELECT`查詢 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


後續的畫面會要求您命名將會建立預存程序。 輸入名稱`Products_SelectByProductID`，按一下 [下一步]。


[![命名新的預存程序 Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**圖 20**： 命名新的預存程序`Products_SelectByProductID`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


在精靈的最後一個步驟可讓我們變更的方法名稱產生，以及指出是否要使用的填滿 DataTable 模式，傳回 DataTable 模式中，或這兩者。 針對這個方法，將 核取，這兩個選項，但重新命名的方法`FillByProductID`和`GetProductByProductID`。 按一下 [下一步] 檢視的精靈會執行，然後按一下 [完成] 以完成精靈步驟摘要。


[![重新命名為 FillByProductID 和 GetProductByProductID 的 TableAdapter 的方法](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**圖 21**： 重新命名的 TableAdapter 的方法`FillByProductID`並`GetProductByProductID`([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


完成精靈之後，TableAdapter 有可用的新方法`GetProductByProductID(productID)`，叫用時，會執行`Products_SelectByProductID`儲存剛才建立的程序。 請花一點時間深入探索 預存程序 資料夾，並開啟檢視這個新的預存程序從 伺服器總管`Products_SelectByProductID`（如果您沒有看到它，以滑鼠右鍵按一下 預存程序 資料夾及選擇 重新整理）。

請注意，`SelectByProductID`預存程序會採用`@ProductID`做為輸入參數，並執行`SELECT`我們在精靈中輸入陳述式。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>步驟 6： 建立商務邏輯層類別

在整個教學課程系列，我們有 strived 維護在其中展示層會完成所有的商務邏輯層 (BLL) 其呼叫的多層式的架構。 為了符合此設計決策，我們首先要建立新的具類型資料集的 BLL 類別，我們才能存取產品資料從展示層。

建立新的類別檔案，名為`ProductsBLLWithSprocs.vb`在`~/App_Code/BLL`資料夾並加入下列程式碼：


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

這個類別會模仿`ProductsBLL`類別從先前的教學課程，但會使用語意`ProductsTableAdapter`並`ProductsDataTable`物件從`NorthwindWithSprocs`資料集。 比方說，而不需要`Imports NorthwindTableAdapters`陳述式的類別檔案開頭`ProductsBLL`當然`ProductsBLLWithSprocs`類別會使用`Imports NorthwindWithSprocsTableAdapters`。 同樣地，`ProductsDataTable`並`ProductsRow`用於這個類別中的物件前面會加上`NorthwindWithSprocs`命名空間。 `ProductsBLLWithSprocs`類別提供兩個資料存取方法，`GetProducts`和`GetProductByProductID`，以及方法以新增、 更新和刪除單一產品執行個體。

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>步驟 7： 使用`NorthwindWithSprocs`從展示層的資料集

此時，我們已經建立使用預存程序來存取和修改基礎資料庫資料的 DAL。 我們也已經建立基本的 BLL 與方法，以擷取所有的產品或特定的產品，以及方法來新增、 更新和刪除的產品。 若要表示圓角本教學課程，可讓 s 建立 ASP.NET 網頁使用 BLL 的`ProductsBLLWithSprocs`顯示、 更新及刪除記錄的類別。

開啟`NewSprocs.aspx`頁面中`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具，並將它命名為拖曳的 GridView `Products`。 從 GridView s 智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別，如圖 22 所示。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**圖 22**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


下拉式清單中的選取索引標籤中有兩個選項，`GetProducts`和`GetProductByProductID`。 因為我們想要在 GridView 中顯示所有產品，選擇`GetProducts`方法。 更新、 插入和刪除索引標籤中每個下拉式清單中只能有其中一種方法。 請確定每個這些下拉式清單都有其適當的方法，選取，然後按一下 [完成]。

ObjectDataSource 精靈完成之後，Visual Studio 會將 BoundFields 及其產品資料欄位的 GridView。 藉由檢查啟用編輯和中的智慧標籤的 啟用刪除選項開啟的 GridView s 內建編輯與刪除功能。


[![此頁面包含與編輯和刪除已啟用支援 GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**圖 23**： 此頁面包含 GridView，以編輯和刪除支援已啟用 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


因為我們 ve 討論 ObjectDataSource s 精靈 中，完成先前的教學課程中 Visual Studio 設定`OldValuesParameterFormatString`屬性的原始\_{0}。 這需要還原成其預設值是{0}才能正常運作的資料修改功能的順序指定我們 BLL 中的方法所需的參數。 因此，請務必設定`OldValuesParameterFormatString`屬性設{0}或從宣告式語法中完全移除屬性。

完成設定資料來源精靈，開啟編輯和刪除的 GridView、 支援及傳回 ObjectDataSource s 之後`OldValuesParameterFormatString`為其預設值的屬性，您的頁面 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

此時我們可以更流暢的 GridView 自訂編輯介面，以包含驗證，需要`CategoryID`和`SupplierID`資料行轉譯為 dropdownlist 進行，依此類推。 我們也可以新增用戶端確認至 [刪除] 按鈕，並建議您花時間來實作這些增強功能。 因為在先前的教學課程中討論過這些主題，不過，我們將不討論它們一次這裡。

不論是否增強 GridView 與否，測試頁面的核心功能，在瀏覽器中。 如圖 24 所示，頁面會列出中的 GridView 會提供每個資料列編輯和刪除功能的產品。


[![產品可以檢視、 編輯和刪除從 GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**圖 24**: 產品可以進行檢視、 編輯，並從 GridView 的已刪除 ([按一下以檢視完整大小的影像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>總結

在 輸入資料集 Tableadapter 可以存取資料，使用特定 SQL 陳述式從資料庫或透過預存程序。 當使用預存程序，其中一個現有的預存程序可用，或可以指示 [Tabaleadapter 精靈] 建立新的預存程序根據`SELECT`查詢。 在本教學課程中，我們會探討如何以自動為我們建立的預存程序。

同時自動產生可協助節省時間的預存程序，有某些情況下，精靈不 t 所建立的預存程序配合什麼我們會建立自己。 其中一個範例是`Products_Update`預存程序，必須同時`@Original_ProductID`並`@ProductID`即使輸入參數`@Original_ProductID`參數，則非必要。

在許多情況下，可能已建立的預存程序，或我們可能想要以手動方式，以便有更精細的程度的控制能力的預存程序的命令來建置。 在任一情況下，我們會想要指示要為其方法使用現有的預存程序的 TableAdapter。 我們應該會看到如何在下一個教學課程中完成這項作業。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建立和維護預存程序](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [從預存程序中擷取純量資料](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server 預存程序的基本概念](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [預存程序： 概觀](http://www.sqlteam.com/item.asp?ItemID=563)
- [撰寫預存程序](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Geisenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [下一頁](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
