---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: 更新要使用的 TableAdapter 聯結 (VB) |Microsoft 文件
author: rick-anderson
description: 使用資料庫時通常會要求資料，則會分散到多個資料表。 若要從兩個不同資料表中擷取資料我們可以使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>更新要使用的 TableAdapter 聯結 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip)或[下載 PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> 使用資料庫時通常會要求資料，則會分散到多個資料表。 從兩個不同資料表擷取資料，我們可以使用相互關聯子查詢或聯結作業。 在本教學課程比較相互關聯子查詢和聯結語法之前查看如何建立在其主要的查詢包含聯結的 TableAdapter。


## <a name="introduction"></a>簡介

與關聯式資料庫我們想要使用的資料通常橫跨多個資料表。 例如，顯示產品資訊時我們可能想要列出每個產品 s 對應的分類和供應商 s 的名稱。 `Products`資料表有`CategoryID`和`SupplierID`值，但實際的類別目錄] 和 [供應商名稱位於`Categories`和`Suppliers`分別資料表。

若要從另一個相關的資料表擷取資訊，我們可以使用*相互關聯子查詢*或`JOIN` *s*。 相互關聯子查詢是巢狀`SELECT`參考外部查詢中的資料行的查詢。 例如，在[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中我們使用中的兩個相互關聯子查詢`ProductsTableAdapter`s 主要查詢來傳回每個產品類別目錄] 和 [供應商名稱。 A`JOIN`是合併兩個不同資料表的相關資料列的 SQL 建構。 我們使用`JOIN`中[查詢資料與 SqlDataSource 控制項](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md)教學課程，以顯示與每個產品類別目錄資訊。

我們已從使用 abstained 原因`JOIN`與 Tableadapter 是由於在 TableAdapter 的精靈中自動產生對應的限制`INSERT`， `UPDATE`，和`DELETE`陳述式。 更具體來說，如果 TableAdapter s 主查詢包含任何`JOIN`s，TableAdapter 無法自動建立臨機操作 SQL 陳述式或預存程序其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。

在此教學課程中我們將會短暫比較和對比相互關聯子查詢和`JOIN`s 之前瀏覽如何建立包含 TableAdapter`JOIN`其主查詢。

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>比較與對照相互關聯子查詢和`JOIN`s

請記得，`ProductsTableAdapter`中的第一個教學課程中建立`Northwind`資料集使用相互關聯子查詢，以回復每個產品 s 對應類別目錄] 和 [供應商的名稱。 `ProductsTableAdapter` s 主查詢如下所示。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

這兩個相互關聯子查詢-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)`和`(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-會`SELECT`傳回每項產品的單一值做為另一個資料行會在外部查詢`SELECT`陳述式 s 資料行清單。

或者，`JOIN`可以用來傳回每個產品 s 供應商和類別名稱。 下列查詢會傳回相同的輸出為上述其中一個，但是會使用`JOIN`s 取代子查詢：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

A`JOIN`合併記錄從一個資料表，根據一些準則的另一個資料表內的記錄。 在上述查詢中，例如`LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID`指示合併每個 SQL Server 產品與分類的記錄會記錄其`CategoryID`值符合產品的`CategoryID`值。 合併的結果，可讓我們使用對應的類別目錄欄位，針對每個產品 (例如`CategoryName`)。

> [!NOTE]
> `JOIN` s 常用於查詢關聯式資料庫中的資料時。 如果您是新手`JOIN`語法或需要複習有點其使用情形，d 建議[SQL Join 教學課程](http://www.w3schools.com/sql/sql_join.asp)在[W3 學校](http://www.w3schools.com/)。 也值得讀取會[`JOIN`基礎](https://msdn.microsoft.com/library/ms191517.aspx)和[子查詢基礎觀念](https://msdn.microsoft.com/library/ms189575.aspx)區段[SQL 線上叢書 》](https://msdn.microsoft.com/library/ms130214.aspx)。


因為`JOIN`s 和相互關聯子查詢可以同時用來從其他資料表擷取相關的資料，許多開發人員會一直突破其標頭，並想要使用哪個方法。 所有 SQL 討論我 ve 談到說大致上相同的作業，它規定 t 真的很重要取向因為 SQL Server 會產生大致上相同的執行計畫。 然後，他們的建議，是使用您和小組是最熟悉的技巧。 它需要注意的是之後 imparting 這項建議, 這些專家立即 express 其喜好設定`JOIN`s 透過相互關聯子查詢。

建置時使用具類型資料集的資料存取層，這些工具時效果更好使用子查詢。 特別是，TableAdapter 的精靈不會自動-產生對應`INSERT`， `UPDATE`，和`DELETE`陳述式，如果主要的查詢包含任何`JOIN`s，但將會自動產生這些陳述式時相互關聯子查詢會使用。

若要瀏覽這個缺點，建立暫存輸入中的資料集`~/App_Code/DAL`資料夾。 在 [TableAdapter 組態精靈] 期間選擇使用特定 SQL 陳述式，並輸入以下`SELECT`查詢 （請參閱圖 1）：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![輸入主要的查詢包含聯結](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**圖 1**： 輸入主要的查詢包含`JOIN`s ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


根據預設，TableAdapter 將會自動建立`INSERT`， `UPDATE`，和`DELETE`主要查詢為基礎的陳述式。 如果您按一下 [進階] 按鈕，您可以看到這項功能已啟用。 儘管這項設定，TableAdapter 將無法建立`INSERT`， `UPDATE`，和`DELETE`陳述式因為主要的查詢中包含`JOIN`。


![輸入主要的查詢包含聯結](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**圖 2**： 輸入主要的查詢，其中包含`JOIN`s


按一下 [完成] 5d; 以完成精靈。 此時 dataset 設計工具時，會針對每個欄位中傳回包含在單一 TableAdapter 與資料行的 datatable`SELECT`查詢 s 資料行清單。 這包括`CategoryName`和`SupplierName`，如圖 3 所示。


![DataTable 資料行清單中傳回每個欄位包含資料行](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**圖 3**： 資料表包含資料行，以傳回資料行清單中的每個欄位


TableAdapter 的 DataTable 具有適當的資料行，而缺少的值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 若要確認這一點，按一下設計工具的 TableAdapter 上，然後移至 [屬性] 視窗。 那里，您會看到`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性設定為 （無）。


[![InsertCommand、 UpdateCommand 和 DeleteCommand 屬性設定為 （無）](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**圖 4**: `InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性設定為 （無） ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


若要解決這個缺點，我們可以手動提供 SQL 陳述式和參數`InsertCommand`， `UpdateCommand`，和`DeleteCommand`透過 [屬性] 視窗的內容。 或者，我們可以藉由設定 TableAdapter s 主查詢，以開始*不*包含任何`JOIN`s。 這可讓`INSERT`， `UPDATE`，和`DELETE`要為我們自動產生陳述式。 完成精靈之後，我們無法以手動方式更新 tableadapter`SelectCommand`從 [屬性] 視窗，使其包含`JOIN`語法。

當這種方法，是很容易損毀時使用特定 SQL 查詢，因為每當 TableAdapter s 主查詢已重新設定完成精靈，自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式會重新建立。 這表示所有自訂之後做便會遺失我們在 TableAdapter 上以滑鼠右鍵按一下、 從內容功能表中，選擇設定和完成精靈一次。

自動產生的 tableadapter 的損毀`INSERT`， `UPDATE`，和`DELETE`陳述式即幸運的是，限制為特定 SQL 陳述式。 如果您的 TableAdapter 會使用預存程序，您可以自訂`SelectCommand`， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`預存程序，並重新執行而不必擔心會預存程序的 TableAdapter 組態精靈修改。

接下來幾個步驟，我們將建立的 TableAdapter，一開始，使用省略任何主查詢`JOIN`s 使對應的 insert、 update 和 delete 預存程序將會自動產生。 我們將會更新`SelectCommand`，因此使用`JOIN`從相關資料表傳回其他資料行。 最後，我們會建立對應的商務邏輯層類別，並示範如何使用 ASP.NET web 網頁中的 TableAdapter。

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>步驟 1： 建立使用簡化的主查詢的 TableAdapter

此教學課程中我們將 TableAdapter 和強型別 DataTable`Employees`資料表中`NorthwindWithSprocs`資料集。 `Employees`資料表包含`ReportsTo`欄位指定`EmployeeID`的員工 s manager。 例如，員工 Anne 葉剛具有`ReportTo`值為 5，這是`EmployeeID`孟。 因此，Anne 向 Steven，其經理報告。 以及報告每位員工的`ReportsTo`值，我們可能也想要擷取其管理員的名稱。 這可以透過`JOIN`。 但使用`JOIN`時一開始建立 TableAdapter 使精靈會自動產生對應的插入、 更新和刪除功能。 因此，我們會先建立其主要的查詢不包含任何 TableAdapter `JOIN` s。 然後，在步驟 2 中，我們將會更新擷取透過 manager s 名稱的主要查詢預存程序`JOIN`。

先開啟`NorthwindWithSprocs`中的資料集`~/App_Code/DAL`資料夾。 以滑鼠右鍵按一下設計工具上，從內容功能表中，選取 [加入] 選項並挑選 TableAdapter 功能表項目。 這會啟動 TableAdapter 組態精靈。 圖 5 描述，會有精靈建立新的預存程序，並按一下 [下一步]。 如建立新的重新整理程式預存程序從 TableAdapter 的精靈，請參閱[建立新預存程序型別資料集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。


[![選取建立新預存程序選項](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**圖 5**： 選取 建立新的預存程序選項 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


使用下列`SELECT`TableAdapter s 主查詢陳述式：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

因為此查詢不包含任何`JOIN`s，TableAdapter 精靈將會自動建立預存程序，該對應`INSERT`， `UPDATE`，和`DELETE`陳述式，以及執行主要的預存程序查詢。

下列步驟可讓我們來命名的 TableAdapter s 預存程序。 使用名稱`Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`，如圖 6 中所示。


[![名稱的 TableAdapter s 預存程序](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**圖 6**: TableAdapter s 預存程序的名稱 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


最後一個步驟，提示我們 TableAdapter 的方法的名稱。 使用`Fill`和`GetEmployees`做為方法名稱。 另外請務必保留更新直接傳送到資料庫 (GenerateDBDirectMethods) checkbox 已核取的建立方法。


[![名稱的 TableAdapter 的方法填滿和 GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**圖 7**： 命名 tableadapter 方法`Fill`和`GetEmployees`([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


完成精靈之後，請花一點時間檢查資料庫中的預存程序。 您應該會看到四個新的： `Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`。 接下來，檢查`EmployeesDataTable`和`EmployeesTableAdapter`剛建立。 資料表包含每個欄位的主查詢所傳回的資料行。 在 TableAdapter 上按一下，然後移至 [屬性] 視窗。 那里，您會看到`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性已正確設定呼叫的對應的預存程序。


[![TableAdapter 包含插入、 更新和刪除功能](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**圖 8**: TableAdapter 包含插入、 更新和刪除功能 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


插入、 更新和刪除自動建立的預存程序和`InsertCommand`， `UpdateCommand`，和`DeleteCommand`正確設定的屬性，我們已準備好自訂`SelectCommand`s 預存程序以傳回其他每個員工 s manager 的相關資訊。 具體而言，我們需要更新`Employees_Select`預存程序使用`JOIN`並傳回 manager s`FirstName`和`LastName`值。 更新預存程序之後，我們必須更新，使其包含下列其他資料行的 DataTable。 我們將會處理這兩項工作在步驟 2 和 3。

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>步驟 2： 自訂預存程序，以包含`JOIN`

移至 [伺服器總管] 中，向下鑽研至 Northwind 資料庫 s 預存程序資料夾中，並開啟開始`Employees_Select`預存程序。 如果看不到此預存程序，請以滑鼠右鍵按一下 預存程序 資料夾，然後選擇 重新整理。 更新預存程序，讓它使用`LEFT JOIN`先傳回 manager s 和姓氏：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

在更新之後，`SELECT`陳述式中，移至 檔案 功能表，然後選取 儲存的變更儲存`Employees_Select`。 或者，您可以按一下工具列中的 [儲存] 圖示，或按 Ctrl + S。 儲存您的變更之後，以滑鼠右鍵按一下`Employees_Select`預存程序，在 伺服器總管，然後選擇 執行。 這將會執行預存程序，並在 [輸出] 視窗中顯示其結果 （請參閱圖 9）。


[![預存程序結果會顯示在 [輸出] 視窗](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**圖 9**: 預存程序結果會顯示在 [輸出] 視窗 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>步驟 3： 更新 DataTable s 資料行

此時，`Employees_Select`預存程序傳回`ManagerFirstName`和`ManagerLastName`值，但`EmployeesDataTable`缺少這些資料行。 這些遺漏的資料行可以加入至資料表中有兩種：

- **手動**-DataSet 設計工具中資料表上按一下滑鼠右鍵並從 新增 功能表中，選擇 資料行。 然後，您可以命名資料行，並據以設定其屬性。
- **自動**-TableAdapter 組態精靈將會更新以反映所傳回的欄位的 DataTable s 資料行`SelectCommand`預存程序。 當使用特定 SQL 陳述式時，也會移除精靈`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性後的`SelectCommand`現在包含`JOIN`。 但當使用預存程序，這些命令屬性保持不變。

我們已在先前的教學課程，包括瀏覽以手動方式加入 DataTable 資料行[主要/詳細說明使用主要記錄項目符號清單與詳細資料 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)和[上傳檔案](../working-with-binary-files/uploading-files-vb.md)，我們會我們的下一個教學課程中查看此程序一次的更多詳細資料。 不過，本教學課程，可讓 s 使用透過 TableAdapter 組態精靈自動的方法。

以滑鼠右鍵按一下啟動`EmployeesTableAdapter`，然後從內容功能表中選取設定。 這會開啟 TableAdapter 組態精靈中，列出用來選取、 插入、 更新和刪除，以及它們的傳回值和參數 （如果有的話） 的預存程序。 圖 10 顯示此精靈。 這裡我們可以看到`Employees_Select`預存程序現在會傳回`ManagerFirstName`和`ManagerLastName`欄位。


[![精靈顯示更新的資料行清單，如 Employees_Select 預存程序](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**圖 10**： 精靈會顯示更新的資料行清單的`Employees_Select`預存程序 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


完成精靈，按一下 [完成]。 在 DataSet 設計工具中，以傳回`EmployeesDataTable`包含兩個資料行：`ManagerFirstName`和`ManagerLastName`。


[![EmployeesDataTable 包含兩個新的資料行](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**圖 11**:`EmployeesDataTable`包含兩個新資料行 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


為了說明，更新`Employees_Select`預存程序作用中，插入、 更新和刪除的 tableadapter 的功能仍運作正常，讓建立網頁，可讓使用者檢視和刪除的員工。 我們建立這樣的頁面之前，不過，我們必須先建立新的類別中的員工所使用的商務邏輯層`NorthwindWithSprocs`資料集。 在步驟 4 中，我們將建立`EmployeesBLLWithSprocs`類別。 在步驟 5 中，我們將使用這個類別，從 ASP.NET 頁面。

## <a name="step-4-implementing-the-business-logic-layer"></a>步驟 4： 實作商務邏輯層

建立新的類別檔案中`~/App_Code/BLL`資料夾名為`EmployeesBLLWithSprocs.vb`。 這個類別會模擬的現有語意`EmployeesBLL`類別，這個新其中一個提供較少的方法，並使用`NorthwindWithSprocs`資料集 (而不是`Northwind`資料集)。 將下列程式碼加入 `EmployeesBLLWithSprocs` 類別。


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

`EmployeesBLLWithSprocs`類別 s`Adapter`屬性傳回的執行個體`NorthwindWithSprocs`dataset `EmployeesTableAdapter`。 這由類別 s`GetEmployees`和`DeleteEmployee`方法。 `GetEmployees`方法呼叫`EmployeesTableAdapter`s 對應`GetEmployees`方法，會叫用`Employees_Select`預存程序，並於其中填入在其結果`EmployeeDataTable`。 `DeleteEmployee`方法同樣會呼叫`EmployeesTableAdapter`s`Delete`方法，會叫用`Employees_Delete`預存程序。

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>步驟 5： 使用展示層中的資料

與`EmployeesBLLWithSprocs`類別完成之後，我們準備好使用透過 ASP.NET 網頁的員工資料。 開啟`JOINs.aspx`頁面`AdvancedDAL`資料夾，然後拖曳 GridView 從工具箱拖曳至設計工具中，設定其`ID`屬性`Employees`。 接下來，GridView s 智慧標籤，從方格繫結至新的 ObjectDataSource 控制項，名為`EmployeesDataSource`。

設定用於 ObjectDataSource`EmployeesBLLWithSprocs`類別，並從 選取和刪除索引標籤，確定`GetEmployees`和`DeleteEmployee`方法會從下拉式清單中選取。 按一下 [完成] 5d; 以完成 ObjectDataSource 的設定。


[![設定使用 EmployeesBLLWithSprocs 類別 ObjectDataSource](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**圖 12**： 設定要使用 ObjectDataSource`EmployeesBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![具有 ObjectDataSource 使用 GetEmployees 和 DeleteEmployee 方法](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**圖 13**: ObjectDataSource 使用`GetEmployees`和`DeleteEmployee`方法 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio 會加入至 GridView BoundField 每個`EmployeesDataTable`s 資料行。 移除所有這些 BoundFields 除了`Title`， `LastName`， `FirstName`， `ManagerFirstName`，和`ManagerLastName`並重新命名`HeaderText`最後四個 BoundFields 姓氏、 名字、 管理員 s First Name 屬性，管理員 s 姓氏，分別。

若要允許使用者從這個頁面刪除員工我們需要做兩件事。 首先，指示 GridView 藉由檢查其智慧標籤的 啟用刪除選項提供刪除功能。 第二，變更 ObjectDataSource s `OldValuesParameterFormatString` ObjectDataSource 精靈設定屬性的值 (`original_{0}`) 為其預設值 (`{0}`)。 進行這些變更之後，您 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

造訪透過瀏覽器來測試頁面。 如圖 14 所示，此頁面會列出每一位員工和他或她管理員 s 的名稱 （假設有的話）。


[![Employees_Select 中的聯結預存程序傳回 Manager 的名稱](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**圖 14**:`JOIN`中`Employees_Select`預存程序會傳回管理員名稱 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


按一下 [刪除] 按鈕會啟動刪除工作流程的執行會達成 「`Employees_Delete`預存程序。 不過，嘗試`DELETE`預存程序的陳述式會失敗，因為外部索引鍵條件約束違規的緣故 （請參閱圖 15）。 具體來說，每位員工中有一或多個記錄`Orders`資料表中，導致無法刪除。


[![刪除外部索引鍵條件約束違規在具有對應的訂單結果的員工](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**圖 15**： 刪除外部索引鍵條件約束違規在具有對應的訂單結果的員工 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


若要讓員工會刪除您可以：

- 更新外部索引鍵條件約束的串聯刪除
- 手動刪除記錄`Orders`employee(s) 您想要刪除的資料表或
- 更新`Employees_Delete`預存程序，先刪除相關的記錄從`Orders`資料表，然後再刪除`Employees`記錄。 我們討論過這項技巧[使用現有預存程序型別資料集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。

我將保留此為一項工作讀取器。

## <a name="summary"></a>總結

當使用關聯式資料庫，它是很常見的查詢，以提取其資料從多個關聯的資料表。 相互關聯子查詢和`JOIN`s 提供兩個不同的技術存取資料，從查詢中的相關資料表。 中最常進行先前的教學課程使用的相互關聯子查詢，因為 TableAdapter 無法自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式中的查詢涉及`JOIN`s。 雖然使用 TableAdapter 組態精靈完成時，將覆寫任何自訂的特定 SQL 陳述式時，可以手動提供這些值。

幸運的是，使用預存程序所建立的 Tableadapter 不會發生與使用特定 SQL 陳述式建立相同的損毀。 因此，就可以建立其主查詢使用的 TableAdapter`JOIN`時使用預存程序。 在此教學課程中我們可了解如何建立這類的 TableAdapter。 我們已開始使用`JOIN`-小於`SELECT`使對應的 insert、 update 和 delete 預存程序會自動建立查詢的 TableAdapter s 主查詢。 TableAdapter s 初始設定完成後，我們擴大`SelectCommand`預存程序使用`JOIN`並重新執行 TableAdapter 組態精靈以更新`EmployeesDataTable`s 資料行。

重新執行 TableAdapter 組態精靈會自動更新`EmployeesDataTable`以反映所傳回的資料欄位的資料行`Employees_Select`預存程序。 或者，我們可以加入這些資料行手動 DataTable。 我們將探討手動加入資料行至資料表，在下一個教學課程。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Hilton Geisenow、 David Suru 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [下一頁](adding-additional-datatable-columns-vb.md)
