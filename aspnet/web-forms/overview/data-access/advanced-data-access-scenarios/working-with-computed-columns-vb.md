---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: 使用計算資料行 (VB) |Microsoft 文件
author: rick-anderson
description: 建立資料庫資料表時，Microsoft SQL Server 可讓您定義其值從運算式計算的計算資料行，通常 referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 04b39902aae05d815eb11ec7b7163988d017f78c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877199"
---
<a name="working-with-computed-columns-vb"></a>使用計算資料行 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip)或[下載 PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> 建立資料庫資料表時，Microsoft SQL Server 可讓您定義其值從通常參考相同的資料庫記錄中的其他值的運算式計算的計算資料行。 這類值都是唯讀，在資料庫中，使用 Tableadapter 時需要特別考量。 在本教學課程中，我們了解如何符合計算資料行所帶來的挑戰。


## <a name="introduction"></a>簡介

Microsoft SQL Server 允許*[計算資料行](https://msdn.microsoft.com/library/ms191250.aspx)*，這些是通常來自相同資料表其他資料行參考值的運算式會計算其值的資料行。 例如，追蹤資料模型的時間可能有資料表，名為`ServiceLog`與資料行包括`ServicePerformed`， `EmployeeID`， `Rate`，和`Duration`，和其他項目。 雖然金額每個服務項目 （要乘以的持續時間的速率） 無法計算透過網頁或其他的程式設計介面，可能會比較方便包括中的資料行`ServiceLog`資料表名為`AmountDue`，回報這資訊。 無法建立此資料行，做為一般的資料行，但它需要隨時更新`Rate`或`Duration`變更的資料行值。 更好的方法是，建立`AmountDue`使用運算式的計算資料行的資料行`Rate * Duration`。 這樣做會導致 SQL Server，即可自動計算`AmountDue`每當在查詢中參考的資料行值。

計算資料行的值取決於運算式，因為這類資料行是唯讀，因此無法值指派給他們在`INSERT`或`UPDATE`陳述式。 不過，當計算資料行屬於主查詢使用特定 SQL 陳述式的 tableadapter，它們會自動包含在自動產生`INSERT`和`UPDATE`陳述式。 因此，tableadapter`INSERT`和`UPDATE`查詢和`InsertCommand`和`UpdateCommand`必須更新屬性，來移除任何計算資料行的參考。

使用的一項挑戰的計算資料行，以使用特定 SQL 陳述式的 TableAdapter 是 tableadapter`INSERT`和`UPDATE`查詢就會自動重新產生任何 TableAdapter 組態精靈完成時的時間。 因此，計算資料行中手動移除從`INSERT`和`UPDATE`查詢將會重新顯示，如果重新執行精靈。 雖然使用預存程序的 Tableadapter 不遭受此損毀，他們擁有自己 quirks 我們解決在步驟 3 中。

在本教學課程中，我們將加入計算資料行`Suppliers`Northwind 資料庫中資料表，然後再建立對應的 TableAdapter，才能使用這個資料表，而且其計算資料行。 我們將會使用預存程序而不是特定 SQL 陳述式，以便使用 TableAdapter 組態精靈時，我們的自訂項目和不 t 遺失我們 TableAdapter。

Let s 開始 ！

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>步驟 1： 加入計算資料行`Suppliers`資料表

Northwind 資料庫沒有任何計算資料行，因此我們必須加入一個自己。 針對本教學課程將加入至計算資料行的 s`Suppliers`資料表稱為`FullContactName`以下列格式傳回連絡人的名稱、 標題和它們適用於公司： `ContactName` (`ContactTitle`， `CompanyName`)。 此計算可能在報表中使用資料行，當顯示供應商的相關資訊。

先開啟`Suppliers`資料表定義，以滑鼠右鍵按一下`Suppliers`資料表在伺服器總管] 中，並從內容功能表中選擇 [開啟資料表定義。 這就會顯示在資料表和其屬性，例如其資料類型的資料行，無論它們允許`NULL`s，依此類推。 若要加入計算資料行，啟動在資料表定義輸入資料行的名稱。 接下來，在資料行屬性 視窗中的計算資料行規格 區段下方的 (Formula) 文字方塊中輸入其運算式 （請參閱圖 1）。 計算資料行的名稱`FullContactName`並使用下列運算式：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

請注意在 SQL 中，可以串連字串使用`+`運算子。 `CASE`可以用來在傳統的程式設計語言中的條件陳述式。 上面的運算式中`CASE`陳述式可以讀取為： 如果`ContactTitle`不`NULL`接著輸出`ContactTitle`以逗號，否則為串連值發出做任何動作。 如需詳細資訊的實用性`CASE`陳述式，請參閱[SQL 電源`CASE`陳述式](http://www.4guysfromrolla.com/webtech/102704-1.shtml)。

> [!NOTE]
> 而不是使用`CASE`以下陳述式，我們原也可以另外使用`ISNULL(ContactTitle, '')`。 [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) 傳回*checkExpression*如果非 NULL，否則它會傳回*replacementValue*。 而是`ISNULL`或`CASE`運作在本例中，有更複雜的案例其中的彈性`CASE`陳述式就無法比對`ISNULL`。


新增此計算資料行後您的畫面看起來應該像螢幕擷取畫面中圖 1。


[![加入名為 FullContactName 至供應商資料表的計算資料行](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**圖 1**： 加入計算資料行名為`FullContactName`至`Suppliers`資料表 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image3.png))


命名計算資料行和輸入的運算式之後儲存變更到資料表上，依序按一下工具列中的 儲存 圖示，請按 Ctrl + S、 或移至 檔案 功能表，然後選取 儲存`Suppliers`。

儲存資料表應該重新整理伺服器總管 中，包括在剛加入的資料行`Suppliers`資料表 s 資料行清單。 此外，在 [(Formula)] 文字方塊中輸入的運算式會自動調整去除不必要的空白、 資料行名稱使用方括號括住的對等運算式 (`[]`)，並包含括號，以更明確地顯示作業的順序：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

如需有關 Microsoft SQL Server 中的計算資料行的詳細資訊，請參閱[技術文件](https://msdn.microsoft.com/library/ms191250.aspx)。 也請參閱[How to: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx)如建立計算資料行的逐步解說。

> [!NOTE]
> 根據預設，並未實際儲存在資料表中計算資料行，但會改為重新計算，每次在查詢中參考這些。 不過，藉由檢查 保存核取方塊，您可以指示來實際儲存在資料表中的計算資料行的 SQL Server。 這樣做可以改善查詢效能的使用中的計算資料行值的計算資料行上建立索引可讓其`WHERE`子句。 請參閱[在計算資料行上建立索引](https://msdn.microsoft.com/library/ms189292.aspx)如需詳細資訊。


## <a name="step-2-viewing-the-computed-column-s-values"></a>步驟 2： 檢視計算資料行的值

O d e s 資料存取層在開始工作之前，花點時間檢視`FullContactName`值。 從 [伺服器總管] 中，以滑鼠右鍵按一下`Suppliers`資料表名稱，並從內容功能表中選擇新的查詢。 這會顯示 [查詢] 視窗，提示我們選取要包含在查詢中的資料表。 新增`Suppliers`資料表，並按一下 [關閉]。 接下來，檢查`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`[Suppliers] 資料表中的資料行。 最後，按一下 [執行查詢並檢視結果] 工具列中的紅色驚嘆號圖示。

如圖 2 所示，結果會包含`FullContactName`，列出`CompanyName`， `ContactName`，和`ContactTitle`使用格式的資料行`ContactName`(`ContactTitle`， `CompanyName`)。


[![FullContactName 使用格式 ContactName （連絡人職稱、 公司名稱）](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**圖 2**:`FullContactName`使用格式`ContactName`(`ContactTitle`， `CompanyName`) ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>步驟 3： 加入`SuppliersTableAdapter`資料存取層

為了能夠使用我們的應用程式中的供應商資訊，我們需要先建立我們 DAL TableAdapter，並 DataTable。 在理想情況下，達成這點會使用相同的簡單步驟，檢查先前的教學課程。 不過，計算資料行的使用會帶來幾縐折的程度，有些討論。

如果您將會使用特定 SQL 陳述式的 TableAdapter，您只可以在 TableAdapter s 主查詢，透過 TableAdapter 組態精靈中，包含計算資料行。 這項目，不過，將會自動產生`INSERT`和`UPDATE`包含計算資料行的陳述式。 如果您嘗試執行其中一種方法，`SqlException`訊息資料行與*ColumnName*無法修改，因為它是計算資料行或是 UNION 運算子的結果將會擲回。 雖然`INSERT`和`UPDATE`陳述式可以透過 tableadapter 手動調整`InsertCommand`和`UpdateCommand`屬性，這些自訂將會遺失 TableAdapter 組態精靈時重新執行。

使用特定 SQL 陳述式的 Tableadapter 的損毀，因為建議，我們會使用預存程序時使用計算資料行。 如果您使用現有的預存程序，只要設定 TableAdapter 中所述[使用現有預存程序型別資料集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。 如果您有 TableAdapter 精靈為您建立的預存程序，不過，務必一開始略過從主查詢的任何計算資料行。 如果您主要的查詢中包含計算資料行，TableAdapter 組態精靈會通知您，完成時，它無法建立對應的預存程序。 簡單地說，我們需要一開始設定使用計算的資料行可用主查詢的 TableAdapter 手動更新對應的預存程序和 tableadapter`SelectCommand`包含計算資料行。 這個方法很類似於使用中的一個[更新要使用的 TableAdapter](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s*教學課程。

本教學課程，可讓 s 加入新的 TableAdapter，並讓它自動為我們建立預存程序。 因此，我們必須將一開始省略`FullContactName`從主查詢的計算資料行。

先開啟`NorthwindWithSprocs`中的資料集`~/App_Code/DAL`資料夾。 以滑鼠右鍵按一下設計工具中，並從內容功能表中，選擇 加入新的 TableAdapter。 這會啟動 TableAdapter 組態精靈。 若要查詢資料，從指定的資料庫 (`NORTHWNDConnectionString`從`Web.config`)，按一下 [下一步]。 因為我們尚未建立任何預存程序進行查詢或修改`Suppliers`資料表中，選取 建立新的預存程序選項，讓精靈將為我們建立，並按一下 下一步。


[![選擇建立新預存程序選項](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**圖 3**： 選擇建立新預存程序選項 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image9.png))


後續步驟提示我們主查詢。 輸入下列查詢會傳回`SupplierID`， `CompanyName`， `ContactName`，和`ContactTitle`每個供應商的資料行。 請注意，此查詢會故意省略計算資料行 (`FullContactName`); 我們將會更新對應預存程序的步驟 4 中包含此資料行。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

之後輸入主要的查詢，並按一下 下一步，精靈會讓我們將會產生四個預存程序的名稱。 這些預存程序的名稱`Suppliers_Select`， `Suppliers_Insert`， `Suppliers_Update`，和`Suppliers_Delete`，如圖 4 所示。


[![自訂的自動產生的預存程序的名稱](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**圖 4**： 自訂自動產生預存程序的名稱 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image12.png))


下一個步驟中，精靈可讓我們名稱 TableAdapter 的方法，並指定用來存取及更新資料的模式。 將所有的三個核取方塊已核取，但重新命名`GetData`方法`GetSuppliers`。 按一下 [完成] 5d; 以完成精靈。


[![重新命名 GetSuppliers GetData 方法](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**圖 5**： 重新命名`GetData`方法`GetSuppliers`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image15.png))


時按一下 [完成]，精靈會建立四個預存程序，並將 TableAdapter 和對應的資料表加入至型別資料集。

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>步驟 4： 在 TableAdapter s 主查詢中包括計算資料行

我們現在需要更新 TableAdapter 和要包含的步驟 3 中建立 DataTable`FullContactName`計算資料行。 這牽涉到兩個步驟：

1. 更新`Suppliers_Select`預存程序傳回`FullContactName`計算資料行，以及
2. 更新以包含相對應的 DataTable`FullContactName`資料行。

開始瀏覽至 [伺服器總管] 中，並向下切入至 [預存程序] 資料夾。 開啟`Suppliers_Select`預存程序和更新`SELECT`查詢來包含`FullContactName`計算資料行：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

儲存預存程序的變更，按一下工具列中的 儲存 圖示、 按下 Ctrl + S 鍵，或選擇 儲存`Suppliers_Select`從 檔案 功能表選項。

接下來，返回 DataSet 設計工具，請以滑鼠右鍵按一下`SuppliersTableAdapter`，並從內容功能表選擇 設定。 請注意，`Suppliers_Select`資料行現在包含`FullContactName`其資料行集合中的資料行。


[![執行 TableAdapter 的組態精靈來更新資料表 s 資料行](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**圖 6**： 執行 tableadapter 組態精靈以更新資料表 s 中的資料行 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image18.png))


按一下 [完成] 5d; 以完成精靈。 這會自動加入至對應的資料行`SuppliersDataTable`。 [Tabaleadapter 精靈] 是聰明，可以偵測出`FullContactName`資料行是計算資料行，因此唯讀。 因此，它會設定資料行 s`ReadOnly`屬性`true`。 若要確認這種情況，選取 從資料行`SuppliersDataTable`然後移至 屬性 視窗 （請參閱圖 7）。 請注意，`FullContactName`資料行 s`DataType`和`MaxLength`屬性也會據此設定。


[![FullContactName 資料行標示為唯讀](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**圖 7**:`FullContactName`資料行標示為唯讀 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>步驟 5： 加入`GetSupplierBySupplierID`TableAdapter 方法

在此教學課程中，我們將建立的 ASP.NET 網頁，顯示在可更新方格中的供應商。 在過去的教學課程我們已更新商務邏輯層的單一記錄擷取特定資料錄從做為強型別 DataTable，更新其內容，然後將傳送更新的 DataTable DAL 回將變更傳播至 DAL資料庫中。 若要完成第一個步驟-擷取從 DAL 正在更新的資料錄集我們需要先加入`GetSupplierBySupplierID(supplierID)`dal 的方法。

以滑鼠右鍵按一下`SuppliersTableAdapter`資料集設計中，然後從內容功能表中選擇 加入查詢選項。 我們在步驟 3 中，讓精靈為我們產生新的預存程序，選取 建立新的預存程序選項 （請參閱上一步圖 3 提供此精靈步驟的螢幕擷取畫面）。 由於這個方法會傳回多個資料行的記錄，表示我們想要使用 SQL 查詢的 SELECT 會傳回資料列，然後按一下 [下一步]。


[![選擇 選取會傳回資料列選項](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**圖 8**： 選擇 選取會傳回資料列選項 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image24.png))


後續的步驟會提示的查詢，以使用這個方法。 輸入下列命令，它會傳回相同的資料欄位做為主要的查詢，但特定供應商。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

下一個畫面會詢問我們將會自動產生預存程序的名稱。 這個預存程序的名稱`Suppliers_SelectBySupplierID`，按一下 [下一步]。


[![預存程序 Suppliers_SelectBySupplierID 名稱](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**圖 9**： 命名預存程序`Suppliers_SelectBySupplierID`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image27.png))


最後，精靈的提示給我們的資料存取模式和 tableadapter 所用的方法名稱。 將這兩個核取方塊，選取此選項，但重新命名`FillBy`和`GetDataBy`方法`FillBySupplierID`和`GetSupplierBySupplierID`分別。


[![名稱 TableAdapter 方法 FillBySupplierID 和 GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**圖 10**： 命名 TableAdapter 方法`FillBySupplierID`和`GetSupplierBySupplierID`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image30.png))


按一下 [完成] 5d; 以完成精靈。

## <a name="step-6-creating-the-business-logic-layer"></a>步驟 6： 建立商務邏輯層

我們建立的 ASP.NET 網頁，會使用步驟 1 中建立計算資料行之前，我們需要先在 BLL 中加入對應的方法。 我們的 ASP.NET 頁面上，我們將會在步驟 7 中建立，可讓使用者檢視及編輯供應商。 因此，我們需要我們 BLL 最小值，提供方法來取得所有供應商，另一種更新特定供應商。

建立新的類別檔案命名為`SuppliersBLLWithSprocs`中`~/App_Code/BLL`資料夾並加入下列程式碼：


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

類似其他 BLL 類別`SuppliersBLLWithSprocs`具有`Protected``Adapter`傳回的執行個體的屬性`SuppliersTableAdapter`類別以及兩個`Public`方法：`GetSuppliers`和`UpdateSupplier`。 `GetSuppliers`方法呼叫，並傳回`SuppliersDataTable`由相對應傳回`GetSupplier`資料存取層中的方法。 `UpdateSupplier`方法擷取特定供應商，透過對 DAL s 呼叫正在更新的相關資訊`GetSupplierBySupplierID(supplierID)`方法。 然後更新`CategoryName`， `ContactName`，和`ContactTitle`屬性和這些變更認可到資料庫，藉由呼叫資料存取層 s`Update`方法，傳入修改`SuppliersRow`物件。

> [!NOTE]
> 除了`SupplierID`和`CompanyName`，供應商資料表的所有資料行允許`NULL`值。 因此，如果傳入的`contactName`或`contactTitle`參數`Nothing`我們需要設定對應`ContactName`和`ContactTitle`屬性`NULL`資料庫值使用`SetContactNameNull`和`SetContactTitleNull`方法，分別。


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>步驟 7： 使用從展示層的計算資料行

使用計算資料行加入至`Suppliers`資料表和 DAL 和 BLL 據以更新，我們就可以開始建置適用於 ASP.NET 網頁`FullContactName`計算資料行。 先開啟`ComputedColumns.aspx`頁面`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 GridView。 設定 GridView s`ID`屬性`Suppliers`和其智慧標籤上，從繫結到名為新 ObjectDataSource `SuppliersDataSource`。 設定用於 ObjectDataSource`SuppliersBLLWithSprocs`類別，我們加入在步驟 6 中備份，並按一下 [下一步]。


[![設定使用 SuppliersBLLWithSprocs 類別 ObjectDataSource](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**圖 11**： 設定要使用 ObjectDataSource`SuppliersBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image33.png))


只有兩個方法中定義`SuppliersBLLWithSprocs`類別：`GetSuppliers`和`UpdateSupplier`。 請確認這兩種方法指定在 選取和分別更新索引標籤上，按一下 完成 以完成 ObjectDataSource 設定。

完成 資料來源組態精靈的詳細資訊，Visual Studio 會將 BoundField 之每個傳回的資料欄位。 移除`SupplierID`BoundField 並變更`HeaderText`屬性`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`BoundFields 公司、 Contact Name、 標題和完整的連絡人名稱，以分別。 從智慧標籤，檢查 啟用編輯核取方塊以開啟 GridView s 內建編輯的功能。

除了新增 BoundFields 至 GridView，完成 資料來源精靈也會設定 ObjectDataSource s Visual Studio`OldValuesParameterFormatString`屬性至原始\_{0}。 還原回此設定為其預設值，{0}。

之後的 GridView 和 ObjectDataSource 進行這些編輯，其宣告式標記看起來應該如下所示：


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

接下來，請造訪此頁面，透過瀏覽器。 如圖 12 所示，每個供應商出現在一個方格，其中包含`FullContactName`資料行，只要其他三個資料行串連後得出其值格式化為`ContactName`(`ContactTitle`， `CompanyName`)。


[![方格會列出每個供應商](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**圖 12**： 方格會列出每個供應商 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image36.png))


針對特定供應商導致回傳，和該資料列呈現中，按一下 [編輯] 按鈕 （請參閱圖 13） 其編輯介面。 前三個資料行轉譯中其編輯介面的預設值-將 TextBox 控制項`Text`屬性設定為資料欄位的值。 `FullContactName`資料行，不過，仍然維持為文字。 當 BoundFields 已加入至資料來源組態精靈中，完成 GridView `FullContactName` BoundField s`ReadOnly`屬性設定為`True`因為對應`FullContactName`中的資料行`SuppliersDataTable`具有其`ReadOnly`屬性設定為`True`。 步驟 4 中註明`FullContactName`s`ReadOnly`屬性設定為`True`因為 TableAdapter 偵測到資料行是計算資料行。


[![FullContactName 資料行是不可編輯](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**圖 13**:`FullContactName`資料行是無法編輯 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image39.png))


請繼續並更新一或多個可編輯的資料行的值，然後按一下 更新。 請注意如何`FullContactName`的值會自動更新以反映變更。

> [!NOTE]
> GridView 目前使用 BoundFields 可編輯的欄位，導致預設的編輯介面。 因為`CompanyName`是必填欄位，它應該轉換成包含 RequiredFieldValidator TemplateField。 我將保留這為一項工作有興趣的讀取器。 請參閱[新增驗證控制項的編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)教學課程，如需有關將轉換為 TemplateField BoundField 並加入驗證控制項的逐步指示。


## <a name="summary"></a>總結

當定義資料表的結構描述，Microsoft SQL Server 可讓包含的計算資料行。 這些是通常是從同一筆記錄中的其他資料行參考值的運算式會計算其值的資料行。 因為值計算資料行是根據運算式，它們是唯讀且無法指派中的值`INSERT`或`UPDATE`陳述式。 嘗試自動產生對應的 TableAdapter 的主要查詢中使用計算資料行時，這會帶來的挑戰`INSERT`， `UPDATE`，和`DELETE`陳述式。

在本教學課程，我們會討論規避計算資料行所造成的驗證題目的技術。 特別是，我們使用預存程序中我們 TableAdapter 克服固有使用特定 SQL 陳述式的 Tableadapter 中損毀。 當使用 TableAdapter 精靈建立新的預存程序時，很重要，我們有一開始省略任何計算資料行，因為它們的存在可防止資料修改預存程序產生的主查詢。 一開始設定 TableAdapter 之後，其`SelectCommand`預存程序可以改裝包含任何計算資料行。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Hilton Geisenow 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](adding-additional-datatable-columns-vb.md)
> [下一頁](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
