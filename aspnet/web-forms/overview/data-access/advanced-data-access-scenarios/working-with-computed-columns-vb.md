---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: 使用計算資料行 (VB) |Microsoft Docs
author: rick-anderson
description: 建立資料庫資料表時，Microsoft SQL Server 可讓您定義其值會計算從運算式計算資料行，通常 referen...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e489640c9075b3443725ddd776ca08fd5569232
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830982"
---
<a name="working-with-computed-columns-vb"></a>使用計算資料行 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip)或[下載 PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> 建立資料庫資料表時，Microsoft SQL Server 可讓您定義計算資料行，其值會計算從通常參考相同資料庫記錄中的其他值的運算式。 這類的值為唯讀，在資料庫中，使用 Tableadapter 時需要特殊考量。 在本教學課程中，我們了解如何符合計算資料行所帶來的挑戰的內容。


## <a name="introduction"></a>簡介

Microsoft SQL Server 是用來*[計算資料行](https://msdn.microsoft.com/library/ms191250.aspx)*，這是其值的計算方式，通常參考相同資料表中的其他資料行中的 值運算式的資料行。 舉例來說，追蹤資料模型的時間可能有一個名為資料表`ServiceLog`包括的資料行`ServicePerformed`， `EmployeeID`， `Rate`，和`Duration`，其他項目。 雖然金額每項服務項目 （速率乘以持續時間） 可能會計算透過網頁或其他程式設計介面，則可能包含的資料行中方便`ServiceLog`名為資料表`AmountDue`，回報這資訊。 無法建立此資料行，做為一般的資料行，但它必須隨時更新`Rate`或`Duration`變更的資料行值。 好的做法是讓`AmountDue`資料行使用運算式的計算資料行`Rate * Duration`。 這樣做會導致 SQL Server，即可自動計算`AmountDue`每當它在查詢中參考的資料行值。

計算資料行的值取決於運算式，因為這類資料行是唯讀，因此不能有值指派給在`INSERT`或`UPDATE`陳述式。 不過，當計算資料行是使用特定 SQL 陳述式的 TableAdapter 的主查詢的一部分，它們會自動包含在自動產生`INSERT`和`UPDATE`陳述式。 因此，tableadapter`INSERT`並`UPDATE`查詢並`InsertCommand`和`UpdateCommand`必須更新屬性，以移除任何計算資料行的參考。

使用其中一項挑戰的計算資料行，以使用特定 SQL 陳述式的 tableadapter 是 tableadapter`INSERT`和`UPDATE`查詢就會自動重新產生完成 [TableAdapter 組態精靈] 的任何時間。 因此，計算資料行中手動移除從`INSERT`和`UPDATE`重新執行精靈時，查詢將會重新出現。 雖然使用預存程序的 TableAdapters 不受到這個脆弱度，它們可以我們將在步驟 3 中將自己 quirks。

在本教學課程中，我們將增加的計算資料行`Suppliers`Northwind 資料庫中資料表，然後再建立相對應的 TableAdapter，才能使用這個資料表，而且其計算資料行。 我們必須使用預存程序而不是特定 SQL 陳述式中，讓我們的自訂項目不 t 遺失使用 TableAdapter 組態精靈時我們 TableAdapter。

讓 s 開始 ！

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>步驟 1： 加入計算資料行`Suppliers`資料表

Northwind 資料庫沒有任何計算資料行，因此我們需要新增一個自己。 本教學課程可讓新增計算資料行 s`Suppliers`呼叫的資料表`FullContactName`，以下列格式傳回 s 的連絡人名稱、 標題和它們適用於公司： `ContactName` (`ContactTitle`， `CompanyName`)。 此計算可能會在報表中使用資料行，當顯示供應商的相關資訊。

首先開啟`Suppliers`表格定義，以滑鼠右鍵按一下`Suppliers`伺服器總管] 中的資料表，並從操作功能表中選擇 [開啟資料表定義。 這就會顯示在資料表和其屬性，例如其資料類型的資料行，無論它們可讓`NULL`s，等等。 若要加入計算資料行，啟動在資料表定義輸入資料行的名稱。 接下來，在資料行屬性 視窗中的計算資料行規格 區段下的 (Formula) 文字方塊中輸入其運算式 （請參閱 圖 1）。 計算資料行命名`FullContactName`，並使用下列運算式：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

請注意，字串可以串連在 SQL 中使用`+`運算子。 `CASE`陳述式可以當成傳統程式設計語言中的條件。 在上面的運算式`CASE`陳述式可讀取為： 如果`ContactTitle`不是`NULL`接著輸出`ContactTitle`串連以逗號，否則為值發出執行任何動作。 如需詳細資訊的實用性`CASE`陳述式，請參閱 <<c2> [ 的 SQL Power`CASE`陳述式](http://www.4guysfromrolla.com/webtech/102704-1.shtml)。

> [!NOTE]
> 而不是使用`CASE`以下陳述式，我們也可以另外使用`ISNULL(ContactTitle, '')`。 [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) 會傳回*checkExpression*如果非 NULL，否則會傳回*replacementValue*。 當其中一個`ISNULL`或`CASE`能在這種情況中，有更複雜的案例其中的彈性`CASE`陳述式無法比對`ISNULL`。


新增此計算資料行之後您的畫面看起來應該類似 圖 1 螢幕擷取畫面。


[![加入名為 [Suppliers] 資料表的 FullContactName 計算資料行](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**圖 1**： 加入計算資料行名為`FullContactName`要`Suppliers`資料表 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image3.png))


計算資料行命名，並輸入其運算式之後儲存變更的資料表上，按一下工具列中的 儲存 圖示，按 Ctrl + S，或移至 檔案 功能表並選擇 儲存`Suppliers`。

儲存資料表應該重新整理伺服器總管 中，包括在剛加入的資料行`Suppliers`資料表 s 資料行清單。 此外，[(Formula)] 文字方塊中輸入的運算式將會自動調整，以去除不必要的空白字元，括住資料行名稱使用方括號的對等運算式 (`[]`)，並包含括號，以更明確地顯示作業的順序：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

如需有關 Microsoft SQL Server 中的計算資料行的詳細資訊，請參閱[技術文件](https://msdn.microsoft.com/library/ms191250.aspx)。 也瞧瞧[How to: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx)針對建立計算資料行的逐步解說。

> [!NOTE]
> 根據預設，計算資料行並未實際儲存在資料表中，但會改為重新計算每個查詢中參考這些一次。 不過，藉由檢查 保存核取方塊，您可以指示來實際儲存在資料表中的 計算資料行的 SQL Server。 這樣做可以改善查詢效能的使用中的計算資料行值的計算資料行上建立索引可讓其`WHERE`子句。 請參閱[在計算資料行上建立索引](https://msdn.microsoft.com/library/ms189292.aspx)如需詳細資訊。


## <a name="step-2-viewing-the-computed-column-s-values"></a>步驟 2： 檢視計算資料行的值

在 資料存取層上開始工作之前，可讓 s 花點時間檢視`FullContactName`值。 從 [伺服器總管] 中，以滑鼠右鍵按一下`Suppliers`資料表名稱，並從操作功能表中選擇新的查詢。 這會顯示一個查詢視窗，提示我們選擇要包含在查詢中哪些資料表。 新增`Suppliers`資料表，並按一下 [關閉]。 接下來，檢查`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`[Suppliers] 資料表中的資料行。 最後，按一下 [執行查詢並檢視結果] 工具列中的紅色驚嘆號圖示。

結果如 [圖 2] 所示，包括`FullContactName`，列出`CompanyName`， `ContactName`，以及`ContactTitle`使用格式的資料行`ContactName`(`ContactTitle`， `CompanyName`)。


[![FullContactName 使用格式 ContactName （連絡人職稱、 公司名稱）](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**圖 2**:`FullContactName`會使用格式`ContactName`(`ContactTitle`， `CompanyName`) ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>步驟 3： 加入`SuppliersTableAdapter`資料存取層

若要使用的供應商資訊，在我們的應用程式中，我們需要先建立我們的 DAL 中的 TableAdapter 和 DataTable。 在理想情況下，這會使用來完成相同的簡單步驟，在先前的教學課程中檢查。 不過，計算資料行的使用帶來一些縐折的程度，值得討論。

如果您使用的使用特定 SQL 陳述式的 TableAdapter，您就可以在 TableAdapter s 主查詢，透過 [TableAdapter 組態精靈] 中，只包含計算資料行。 這項目，不過，將會自動產生`INSERT`和`UPDATE`包含計算資料行的陳述式。 如果您嘗試執行其中一種方法中，`SqlException`訊息的資料行*ColumnName*無法修改，因為它是其中一個計算的資料行或等位運算子的結果將會擲回。 雖然`INSERT`並`UPDATE`陳述式可以透過 tableadapter 手動調整`InsertCommand`和`UpdateCommand`屬性，這些自訂項目就會遺失 [TableAdapter 組態精靈] 可重新執行。

由於使用特定 SQL 陳述式的 TableAdapters 的脆弱，建議，我們使用預存程序時使用計算資料行。 如果您使用現有的預存程序，只需要設定 TableAdapter 中所述[使用現有預存程序的輸入資料集 Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。 如果您有為您建立的預存程序 [Tabaleadapter 精靈]，不過，務必一開始略過主查詢的任何計算資料行。 如果您在主要的查詢包含計算資料行，[TableAdapter 組態精靈] 會通知您，完成時，它無法建立對應的預存程序。 簡單地說，我們需要一開始設定使用計算的資料行無主查詢的 TableAdapter，並手動更新對應的預存程序和 tableadapter`SelectCommand`包含計算資料行。 這種方法是中使用類似[更新 TableAdapter 以使用](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s*教學課程。

本教學課程中，可讓 s 新增新的 TableAdapter，並讓它自動為我們建立的預存程序。 因此，我們需要一開始省略`FullContactName`主查詢的計算資料行。

首先開啟`NorthwindWithSprocs`中的資料集`~/App_Code/DAL`資料夾。 以滑鼠右鍵按一下設計工具中，並從操作功能表中，選擇 加入新的 TableAdapter。 這會啟動 [TableAdapter 組態精靈]。 查詢資料，從指定的資料庫 (`NORTHWNDConnectionString`從`Web.config`)，按一下 [下一步]。 因為我們尚未建立任何預存程序的查詢，或修改`Suppliers`資料表中，選取 建立新的預存程序選項，讓精靈將會為我們建立它們，並按一下 下一步。


[![選擇 建立新預存程序選項](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**圖 3**： 選擇 建立新預存程序選項 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image9.png))


後續步驟會提示我們輸入主查詢。 輸入下列查詢會傳回`SupplierID`， `CompanyName`， `ContactName`，和`ContactTitle`每個供應商的資料行。 請注意，此查詢會故意省略計算資料行 (`FullContactName`); 我們將會更新在步驟 4 中包含此資料行對應的預存程序。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

輸入主要的查詢，並按一下 下一步之後, 此精靈可讓我們命名將會產生四個預存程序。 命名這些預存程序`Suppliers_Select`， `Suppliers_Insert`， `Suppliers_Update`，和`Suppliers_Delete`，如 圖 4 所示。


[![自訂的自動產生的預存程序的名稱](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**圖 4**： 自訂自動產生預存程序的名稱 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image12.png))


下一個步驟中，精靈可讓我們命名的 TableAdapter 的方法並指定用來存取及更新資料的模式。 將所有的三個核取方塊已核取，但重新命名`GetData`方法，以`GetSuppliers`。 按一下 完成 以完成精靈。


[![重新命名 GetSuppliers GetData 方法](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**圖 5**： 重新命名`GetData`方法來`GetSuppliers`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image15.png))


時按一下 [完成]，精靈會建立四個預存程序，並將 TableAdapter 和對應的資料表加入至具類型資料集。

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>步驟 4： 在 TableAdapter s 主查詢中包括計算資料行

我們現在需要更新 TableAdapter 和要包含的步驟 3 中建立的 DataTable`FullContactName`計算資料行。 這牽涉到兩個步驟：

1. 更新`Suppliers_Select`預存程序傳回`FullContactName`計算資料行，以及
2. 更新以包含相對應的 DataTable`FullContactName`資料行。

開始瀏覽至 [伺服器總管]，然後向下切入至 [預存程序] 資料夾。 開啟`Suppliers_Select`預存程序，並更新`SELECT`查詢包含`FullContactName`計算資料行：


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

將所做的變更儲存至預存程序，按一下工具列中的 [儲存] 圖示、 按下 Ctrl + S 鍵，或選擇另存`Suppliers_Select`從 [檔案] 功能表選項。

接下來，返回 DataSet 設計工具，請以滑鼠右鍵按一下`SuppliersTableAdapter`，並從操作功能表中選擇設定。 請注意，`Suppliers_Select`資料行現在包含`FullContactName`其資料行集合中的資料行。


[![執行 TableAdapter 的組態精靈 來更新資料表 s 資料行](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**圖 6**： 執行 tableadapter 組態精靈以更新 DataTable s 資料行 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image18.png))


按一下 完成 以完成精靈。 這會自動新增至對應的資料行`SuppliersDataTable`。 [Tabaleadapter 精靈] 是夠聰明，無法偵測到`FullContactName`資料行是計算資料行，因此是唯讀。 因此，它會設定資料行 s`ReadOnly`屬性設`true`。 若要確認這點，選取 從資料行`SuppliersDataTable`，然後移至 屬性 視窗 （請參閱 圖 7）。 請注意，`FullContactName`資料行 s`DataType`和`MaxLength`屬性也會據以設定。


[![FullContactName 資料行標示為唯讀](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**圖 7**:`FullContactName`資料行標示為唯讀 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>步驟 5： 新增`GetSupplierBySupplierID`TableAdapter 方法

在本教學課程中，我們將建立 ASP.NET 網頁，可更新的方格中顯示供應商。 在過去的教學課程我們更新了商務邏輯層中的單一記錄擷取特定的記錄，從做為強型別的 DataTable，更新其屬性，然後將傳送更新的 DataTable DAL 回到來傳播變更至 DAL在資料庫中。 若要完成第一個步驟-擷取從 DAL 更新的記錄-我們必須先新增`GetSupplierBySupplierID(supplierID)`DAL 的方法。

以滑鼠右鍵按一下`SuppliersTableAdapter`資料集設計中，然後從操作功能表中選擇 加入查詢選項。 如同我們在步驟 3 中，讓精靈為我們產生新的預存程序，藉由選取 建立新的預存程序選項 （請參閱上一步 圖 3 提供此精靈步驟的螢幕擷取畫面）。 由於這個方法會傳回具有多個資料行的記錄，指出我們想要使用 SQL 查詢是 SELECT 會傳回資料列，然後按一下 [下一步]。


[![選擇 選取會傳回資料列選項](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**圖 8**： 選擇 選取會傳回資料列選項 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image24.png))


後續步驟會提示我們輸入要用於這個方法的查詢。 輸入下列命令，它會傳回相同的資料欄位做為主要的查詢，但特定供應商。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

下一個畫面會要求我們命名將會自動產生預存程序。 命名此預存程序`Suppliers_SelectBySupplierID`，按一下 [下一步]。


[![命名預存程序 Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**圖 9**： 命名預存程序`Suppliers_SelectBySupplierID`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image27.png))


最後，精靈的提示我們，好讓資料存取模式和要使用的 TableAdapter 方法名稱。 保留已核取，這兩個核取方塊，但重新命名`FillBy`並`GetDataBy`方法來`FillBySupplierID`和`GetSupplierBySupplierID`分別。


[![名稱的 TableAdapter 方法 FillBySupplierID 和 GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**圖 10**： 命名 TableAdapter 方法`FillBySupplierID`並`GetSupplierBySupplierID`([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image30.png))


按一下 完成 以完成精靈。

## <a name="step-6-creating-the-business-logic-layer"></a>步驟 6： 建立商務邏輯層

我們建立的 ASP.NET 網頁，會使用在步驟 1 中建立計算資料行之前，我們首先要到 BLL 中新增對應的方法。 我們 ASP.NET 頁面中，我們將建立在步驟 7 中，可讓使用者檢視及編輯供應商。 因此，我們需要我們以最小值，提供方法來取得所有供應商，另一種更新特定的供應商的 BLL。

建立新的類別檔案，名為`SuppliersBLLWithSprocs`在`~/App_Code/BLL`資料夾並加入下列程式碼：


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

和其他 BLL 類別一樣`SuppliersBLLWithSprocs`已經`Protected``Adapter`傳回的執行個體的屬性`SuppliersTableAdapter`類別以及兩個`Public`方法：`GetSuppliers`並`UpdateSupplier`。 `GetSuppliers`方法呼叫，並傳回`SuppliersDataTable`傳回的對應`GetSupplier`資料存取層中的方法。 `UpdateSupplier`方法會擷取特定供應商透過對 DAL s 的呼叫正在更新的相關資訊`GetSupplierBySupplierID(supplierID)`方法。 然後更新`CategoryName`， `ContactName`，並`ContactTitle`屬性及這些變更認可到資料庫，藉由呼叫 s 中的資料存取層`Update`方法並傳入已修改`SuppliersRow`物件。

> [!NOTE]
> 除了`SupplierID`並`CompanyName`、 [Suppliers] 資料表中的所有資料行允許`NULL`值。 因此，如果傳入的`contactName`或`contactTitle`參數是`Nothing`我們需要設定對應`ContactName`並`ContactTitle`屬性，以`NULL`資料庫值使用`SetContactNameNull`和`SetContactTitleNull`方法，分別。


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>步驟 7： 使用從展示層的計算資料行

若要新增的計算資料行`Suppliers`資料表的 DAL 和 BLL 隨之更新中，我們已準備好建置適用於 ASP.NET 網頁`FullContactName`計算資料行。 首先開啟`ComputedColumns.aspx`頁面中`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳的 GridView。 設定 GridView s`ID`屬性，以`Suppliers`和它的智慧標籤，從繫結至名為新 ObjectDataSource `SuppliersDataSource`。 設定要使用 ObjectDataSource`SuppliersBLLWithSprocs`類別，我們加入備份在步驟 6 中，按一下 [下一步]。


[![設定使用 SuppliersBLLWithSprocs 類別 ObjectDataSource](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**圖 11**： 設定要使用 ObjectDataSource`SuppliersBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image33.png))


只有兩種方法中定義`SuppliersBLLWithSprocs`類別：`GetSuppliers`和`UpdateSupplier`。 請確認這兩種方法會指定在 選取和分別更新索引標籤上，按一下 完成 以完成設定 ObjectDataSource。

完成 資料來源組態精靈的詳細資訊，Visual Studio 會針對每一個傳回的資料欄位加入 BoundField。 移除`SupplierID`BoundField 和變更`HeaderText`的屬性`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`BoundFields 公司、 連絡人名稱、 標題和完整的連絡人名稱，以分別。 從智慧標籤，檢查 啟用編輯 核取方塊以開啟 GridView s 內建編輯功能。

除了新增 BoundFields 至 GridView，完成 [資料來源精靈] 也會設定 ObjectDataSource s 的 Visual Studio`OldValuesParameterFormatString`屬性的原始\_{0}。 還原此設定回其預設值， {0} 。

之後的 GridView 和 ObjectDataSource 進行這些編輯，其宣告式標記看起來應該如下所示：


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

接下來，請瀏覽此頁面，透過瀏覽器。 如 [圖 12] 所示，每個供應商會列在一個方格，其中包含`FullContactName`格式化為的資料行，其值為其他三個資料行的串連`ContactName`(`ContactTitle`， `CompanyName`)。


[![方格中會列出每個供應商](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**圖 12**： 每個供應商方格中列出 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image36.png))


按一下 編輯 按鈕，特定供應商造成回傳，並具有該資料列呈現在其編輯介面 （請參閱 圖 13）。 前三個資料行轉譯中其編輯介面的預設值-TextBox 控制項`Text`屬性設定為資料欄位的值。 `FullContactName`資料行，不過，仍會為文字。 當 BoundFields 已新增至 [資料來源組態精靈] 中，完成 GridView `FullContactName` BoundField s`ReadOnly`屬性設定為`True`因為對應`FullContactName`中的資料行`SuppliersDataTable`具有其`ReadOnly`屬性設定為`True`。 步驟 4 中所述`FullContactName`s`ReadOnly`屬性設定為`True`因為 TableAdapter 偵測到的資料行是計算資料行。


[![FullContactName 資料行是無法編輯](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**圖 13**:`FullContactName`資料行是無法編輯 ([按一下以檢視完整大小的影像](working-with-computed-columns-vb/_static/image39.png))


繼續並更新一或多個可編輯的資料行的值並按一下 [更新]。 請注意如何`FullContactName`的值會自動更新以反映變更。

> [!NOTE]
> GridView 目前使用 BoundFields 可編輯的欄位，導致預設的編輯介面。 因為`CompanyName`欄位是必要的它應該轉換成包含 RequiredFieldValidator TemplateField。 我不要更動此練習中，有興趣的讀者。 請參閱[將驗證控制項加入編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)有關將轉換為 TemplateField BoundField 和新增驗證控制項的逐步指示的教學課程。


## <a name="summary"></a>總結

在定義資料表的結構描述時，Microsoft SQL Server 可讓包含的計算資料行。 這些是通常參考相同的記錄中的其他資料行中的 值運算式會計算其值的資料行。 因為值的計算資料行是根據運算式，它們都是唯讀且無法指派中的值`INSERT`或`UPDATE`陳述式。 嘗試將自動產生對應的 TableAdapter 的主要查詢中使用計算資料行時，這會帶來挑戰`INSERT`， `UPDATE`，和`DELETE`陳述式。

在本教學課程中，我們會討論用來規避計算資料行所帶來的挑戰技巧。 特別是，我們使用預存程序在我們的 TableAdapter 克服使用特定 SQL 陳述式的 TableAdapters 的固有之脆弱度。 使用 TableAdapter 精靈建立新預存程序，很重要，我們有一開始省略任何計算資料行，因為它們的存在可防止資料修改預存程序產生的主查詢。 一開始設定 TableAdapter 之後，其`SelectCommand`可以改裝預存程序，以包含任何計算資料行。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Geisenow 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](adding-additional-datatable-columns-vb.md)
> [下一頁](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
