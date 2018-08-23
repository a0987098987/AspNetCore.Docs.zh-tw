---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: 更新 TableAdapter 以使用 Join (C#) |Microsoft Docs
author: rick-anderson
description: 使用資料庫時通常會要求資料，則會分散到多個資料表。 若要擷取兩個不同的資料表資料的我們可以使用...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: d5b69b502cf650a477b2841c25ad3f1ab5f8da93
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833216"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>更新 TableAdapter 以使用 Join (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip)或[下載 PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> 使用資料庫時通常會要求資料，則會分散到多個資料表。 若要從兩個不同資料表中擷取資料中，我們可以使用相互關聯子查詢或聯結作業。 在本教學課程我們再看看如何建立包含聯結，在其主查詢的 TableAdapter 比較相互關聯子查詢和聯結語法。


## <a name="introduction"></a>簡介

與關聯式資料庫通常是分散多個資料表的我們有興趣使用的資料。 例如，顯示產品資訊時我們可能想要列出每個產品 s 對應類別目錄和供應商 s 的名稱。 `Products`資料表有`CategoryID`並`SupplierID`值，但實際的類別和供應商名稱位於`Categories`和`Suppliers`分別資料表。

若要從另一個相關的資料表擷取資訊，我們可以使用*相互關聯子查詢*或是`JOIN` *s*。 相互關聯子查詢是巢狀`SELECT`參考外部查詢中的資料行的查詢。 例如，在[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中我們使用中的兩個相互關聯子查詢`ProductsTableAdapter`s 主要查詢來傳回每個產品類別目錄和供應商名稱。 A`JOIN`是合併兩個不同資料表的相關資料列的 SQL 建構。 我們使用了`JOIN`中[SqlDataSource 控制項查詢資料](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md)教學課程，以顯示與每個產品類別目錄資訊。

我們已從使用 abstained 的原因`JOIN`與 Tableadapter 是由於 TableAdapter 的精靈自動產生的對應中的限制`INSERT`， `UPDATE`，和`DELETE`陳述式。 更具體來說，如果 TableAdapter s 主查詢包含任何`JOIN`s，TableAdapter 無法自動建立的特定 SQL 陳述式或預存程序及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。

在此教學課程中我們將會簡短地比較和對比相互關聯子查詢及`JOIN`之後再探索如何建立包含的 TableAdapter s`JOIN`其主查詢。

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>比較及對照相互關聯子查詢和`JOIN`s

請記得，`ProductsTableAdapter`中的第一個教學課程中建立`Northwind`資料集使用相互關聯子查詢來重新顯示每個產品 s 對應類別目錄和供應商的名稱。 `ProductsTableAdapter` s 主查詢如下所示。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

這兩個相互關聯子查詢-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)`並`(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-會`SELECT`查詢傳回的每項產品的單一值做為額外的資料行中的外部`SELECT`陳述式 s 資料行清單。

或者，`JOIN`可用來傳回每個產品 s 供應商和類別名稱。 下列查詢會傳回與上述的其中一個，相同的輸出，但使用`JOIN`s 取代子查詢：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A`JOIN`合併從一個資料表記錄與根據一些準則的另一個資料表中的記錄。 在上述查詢中，例如`LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID`會指示 SQL Server 合併每個產品與分類的記錄會記錄其`CategoryID`值符合產品的`CategoryID`值。 合併的結果可讓我們使用對應的類別目錄欄位，針對每個產品 (例如`CategoryName`)。

> [!NOTE]
> `JOIN` 查詢關聯式資料庫中的資料時，通常會使用 s。 如果您是新手`JOIN`語法或需要稍微重溫一下其使用方式，d 建議[SQL Join 的教學課程](http://www.w3schools.com/sql/sql_join.asp)在[W3 學校](http://www.w3schools.com/)。 也值得一讀都[`JOIN`基本概念](https://msdn.microsoft.com/library/ms191517.aspx)並[子查詢基本概念](https://msdn.microsoft.com/library/ms189575.aspx)區段[SQL 線上叢書 》](https://msdn.microsoft.com/library/ms130214.aspx)。


因為`JOIN`s 和相互關聯子查詢都可用來擷取其他資料表中的相關的資料，許多開發人員仍正準備碰上其標頭，並想要使用哪個方法。 所有的 SQL 專家我 ve 談過要說大致上相同的作業，它不 t 是很重要效能方面，SQL Server 將會產生大致上相同的執行計畫。 然後，其建議中，是使用您和您的小組是最熟悉的技巧。 它需要注意的是之後 imparting 這項建議, 這些專家立即 express 其喜好設定`JOIN`s 透過相互關聯子查詢。

建置時使用具類型資料集的資料存取層，這些工具適合使用子查詢時。 特別的是，TableAdapter 的精靈將無法自動產生對應`INSERT`， `UPDATE`，和`DELETE`陳述式，如果主要的查詢包含任何`JOIN`s，但將會自動產生這些陳述式時相互關聯子查詢會使用。

若要探索這個缺點，建立暫存類型中的資料集`~/App_Code/DAL`資料夾。 在 TableAdapter 組態精靈 期間選擇使用特定 SQL 陳述式，並輸入下列`SELECT`查詢 （請參閱 圖 1）：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![輸入主要的查詢包含聯結](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**圖 1**： 輸入主要查詢的 Contains `JOIN` s ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


根據預設，TableAdapter 將會自動建立`INSERT`， `UPDATE`，和`DELETE`主要查詢為基礎的陳述式。 如果您按一下 [進階] 按鈕，您可以看到這項功能已啟用。 儘管這項設定，TableAdapter 將無法建立`INSERT`， `UPDATE`，並`DELETE`陳述式因為主要的查詢中包含`JOIN`。


![輸入主要的查詢包含聯結](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**圖 2**： 輸入主要的查詢，其中包含`JOIN`s


按一下 完成 以完成精靈。 此時您資料集設計工具會包含單一 TableAdapter 與資料行的 datatable 中傳回的欄位的每個`SELECT`查詢 s 資料行清單。 這包括`CategoryName`和`SupplierName`，如 圖 3 所示。


![DataTable 資料行清單中傳回每個欄位包含資料行](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**圖 3**: DataTable 包含資料行，傳回資料行清單中的每個欄位


TableAdapter 的 DataTable 有適當的資料行，缺少值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 若要確認這一點，按一下設計工具的 TableAdapter 上，然後移至 [屬性] 視窗。 那里，您會看到`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性會設為 （無）。


[![InsertCommand、 UpdateCommand 和 DeleteCommand 屬性設定為 （無）](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**圖 4**: `InsertCommand`， `UpdateCommand`，以及`DeleteCommand`屬性設定為 （無） ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


若要解決這個缺點，我們可以手動提供的 SQL 陳述式和參數`InsertCommand`， `UpdateCommand`，和`DeleteCommand`透過 [屬性] 視窗的屬性。 或者，我們可以藉由設定 TableAdapter s 主要查詢，以啟動*未*包含任何`JOIN`s。 這可讓`INSERT`， `UPDATE`，和`DELETE`會為我們自動產生的陳述式。 完成精靈之後，我們無法以手動方式更新 tableadapter`SelectCommand`從 [屬性] 視窗，使其包含`JOIN`語法。

雖然這種方法的運作方式，是非常脆弱重新設定完成精靈，自動產生使用特定 SQL 查詢，因為每當 TableAdapter s 主查詢時`INSERT`， `UPDATE`，和`DELETE`陳述式會重新建立。 這表示我們稍後所做的自訂便會遺失我們 TableAdapter 上以滑鼠右鍵按一下，從操作功能表中選擇設定並再次完成精靈。

自動產生的 tableadapter 的脆弱`INSERT`， `UPDATE`，和`DELETE`陳述式，幸運的是，限制為特定 SQL 陳述式。 如果您的 TableAdapter 會使用預存程序，您可以自訂`SelectCommand`， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`預存程序，並重新執行而不需要擔心預存程序，將會的 TableAdapter 組態精靈修改。

接下來幾個步驟，我們將建立的 TableAdapter，一開始，會使用主要查詢省略任何`JOIN`s，以便對應插入、 更新和刪除預存程序將會自動產生。 我們會接著更新`SelectCommand`，因此使用`JOIN`相關資料表中傳回其他資料行。 最後，我們會建立對應的商務邏輯層類別，並示範如何在 ASP.NET 網頁中使用的 TableAdapter。

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>步驟 1： 建立使用簡化的主查詢的 TableAdapter

本教學課程中，我們將新增 TableAdapter 和強型別的 DataTable`Employees`資料表中`NorthwindWithSprocs`資料集。 `Employees`資料表包含`ReportsTo`欄位指定`EmployeeID`的員工 s manager。 例如，員工 Anne 葉剛已`ReportTo`值為 5，也就是`EmployeeID`孟。 因此，安妮報告 Steven，她的管理員。 以及報告每一位員工的`ReportsTo`值，我們可能也想要擷取其管理員的名稱。 這可以使用來完成`JOIN`。 但使用`JOIN`時一開始建立 TableAdapter 防止精靈自動產生對應的插入、 更新和刪除功能。 因此，我們將先建立其主要的查詢不包含任何 TableAdapter `JOIN` s。 然後，在步驟 2 中，我們將更新主要查詢預存程序，擷取透過管理員的名稱`JOIN`。

首先開啟`NorthwindWithSprocs`中的資料集`~/App_Code/DAL`資料夾。 以滑鼠右鍵按一下設計工具上，從操作功能表中，選取 [新增] 選項，挑選 TableAdapter 的功能表項目。 這會啟動 [TableAdapter 組態精靈]。 如 圖 5 說明，讓精靈建立新的預存程序，並按一下 下一步。 建立新的重新整理程式中，預存程序，從 [TableAdapter] s 精靈，請參閱[建立新預存程序的輸入資料集 Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程。


[![選取 建立新預存程序選項](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**圖 5**： 選取 建立新的預存程序選項 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


使用下列項目`SELECT`TableAdapter s 主查詢的陳述式：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

因為此查詢不包含任何`JOIN`s，[Tabaleadapter 精靈] 會自動建立預存程序，該 id 對應`INSERT`， `UPDATE`，和`DELETE`陳述式，以及執行主要的預存程序查詢。

下列步驟可讓我們來命名的 TableAdapter s 預存程序。 使用名稱`Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`，如 [圖 6] 所示。


[![名稱的 TableAdapter s 預存程序](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**圖 6**: TableAdapter s 預存程序的名稱 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


最後一個步驟會提示我們輸入來命名的 TableAdapter 的方法。 使用`Fill`和`GetEmployees`做為方法名稱。 也請務必將更新直接傳送至資料庫 (GenerateDBDirectMethods) 核取方塊已核取的建立方法。


[![名稱的 TableAdapter 的方法填滿和 GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**圖 7**： 命名 tableadapter 方法`Fill`並`GetEmployees`([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


完成精靈之後，花點時間檢查資料庫中的預存程序。 您應該會看到四個新的： `Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`。 接下來，檢查`EmployeesDataTable`和`EmployeesTableAdapter`剛建立。 DataTable 包含主查詢所傳回的每個欄位的資料行。 TableAdapter 上按一下，然後移至 [屬性] 視窗。 那里，您會看到`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性已正確設定來呼叫對應的預存程序。


[![TableAdapter 包含插入、 更新和刪除功能](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**圖 8**: TableAdapter 包含插入、 更新和刪除功能 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


插入、 更新和刪除自動建立的預存程序和`InsertCommand`， `UpdateCommand`，並`DeleteCommand`正確設定的屬性，我們已準備好自訂`SelectCommand`s 預存程序傳回其他每個員工 s manager 的相關資訊。 具體來說，我們需要更新`Employees_Select`預存程序，使用`JOIN`，並傳回 manager s`FirstName`和`LastName`值。 更新預存程序之後，我們必須更新 DataTable，使其包含這些額外的資料行。 我們將會處理這兩項工作在步驟 2 和 3。

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>步驟 2： 自訂以包含預存程序`JOIN`

開始請先移至 [伺服器總管] 中，向下切入至 [Northwind 資料庫 s 預存程序] 資料夾中，並開啟`Employees_Select`預存程序。 如果看不到此預存程序，以滑鼠右鍵按一下 預存程序 資料夾，並選擇 重新整理。 更新預存程序，使其使用`LEFT JOIN`先傳回 manager s 和姓氏：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

在更新之後`SELECT`陳述式中，移至 檔案 功能表，然後選擇 儲存的變更儲存`Employees_Select`。 或者，您可以按一下工具列中的 [儲存] 圖示，或按 Ctrl + S。 儲存您的變更之後，以滑鼠右鍵按一下`Employees_Select`預存程序，在 伺服器總管，然後選擇 執行。 這會執行預存程序，並在 輸出 視窗中顯示其結果 （請參閱 圖 9）。


[![預存程序結果會顯示在 [輸出] 視窗](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**圖 9**: 預存程序的結果會顯示在 [輸出] 視窗 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>步驟 3： 更新資料表 s 資料行

此時，`Employees_Select`預存程序會傳回`ManagerFirstName`並`ManagerLastName`的值，但`EmployeesDataTable`遺失這些資料行。 這些遺漏的資料行可以加入至 DataTable 中有兩種：

- **以手動方式**-以滑鼠右鍵按一下 DataSet 設計工具中的 DataTable 並從 新增 功能表中，選擇 資料行。 您可以命名資料行，然後據以設定其屬性。
- **自動**-[TableAdapter 組態精靈] 將會更新資料表 s 資料行，以反映所傳回的欄位`SelectCommand`預存程序。 當使用特定 SQL 陳述式，也會移除精靈`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性，因為`SelectCommand`現在包含`JOIN`。 但在使用預存程序時，命令會保留這些屬性保持不變。

我們在先前的教學課程，包括探索了手動加入的 DataTable 資料行[主要/詳細說明使用具有詳細資料 DataList 的主要記錄項目符號清單](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)並[上傳檔案](../working-with-binary-files/uploading-files-cs.md)，而我們也將在我們的下一個教學課程中，查看此程序一次的更多詳細資料。 本教學課程中，不過，讓使用透過 [TableAdapter 組態精靈] 的自動方式的 s。

以滑鼠右鍵按一下啟動`EmployeesTableAdapter`並從內容功能表中選取設定。 這會顯示 TableAdapter 組態精靈，它會列出用於選取、 插入、 更新和刪除，以及它們的傳回值和參數 （如果有的話） 的預存程序。 [圖 10] 顯示此精靈。 這裡我們可以看到`Employees_Select`預存程序現在會傳回`ManagerFirstName`和`ManagerLastName`欄位。


[![精靈顯示更新的資料行清單中，針對 Employees_Select 預存程序](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**圖 10**： 精靈會顯示更新資料行清單，如`Employees_Select`預存程序 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


按一下 [完成] 來完成精靈。 DataSet 設計工具中，返回`EmployeesDataTable`包含兩個額外的資料行：`ManagerFirstName`和`ManagerLastName`。


[![EmployeesDataTable 包含兩個新的資料行](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**圖 11**:`EmployeesDataTable`包含兩個新資料行 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


為了說明，更新`Employees_Select`預存程序作用中，插入、 更新和刪除的 TableAdapter 的功能都仍運作正常，讓建立網頁，可讓使用者檢視和刪除員工。 建立這類頁面之前，不過，我們必須先建立新的類別中的員工所使用的商務邏輯層`NorthwindWithSprocs`資料集。 在步驟 4 中，我們將建立`EmployeesBLLWithSprocs`類別。 在步驟 5 中，我們將使用此類別從 ASP.NET 頁面。

## <a name="step-4-implementing-the-business-logic-layer"></a>步驟 4： 實作商務邏輯層

建立新的類別檔案中`~/App_Code/BLL`名為資料夾`EmployeesBLLWithSprocs.cs`。 這個類別會模仿現有的語意`EmployeesBLL`類別，這個新其中一個提供較少的方法，並使用`NorthwindWithSprocs`資料集 (而不是`Northwind`資料集)。 將下列程式碼加入 `EmployeesBLLWithSprocs` 類別。


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs`類別 s`Adapter`屬性傳回的執行個體`NorthwindWithSprocs`dataset `EmployeesTableAdapter`。 這由類別 s`GetEmployees`和`DeleteEmployee`方法。 `GetEmployees`方法呼叫`EmployeesTableAdapter`s 對應`GetEmployees`方法，它會叫用`Employees_Select`預存程序，並於其中填入在其結果`EmployeeDataTable`。 `DeleteEmployee`方法會同樣地呼叫`EmployeesTableAdapter`s`Delete`方法，它會叫用`Employees_Delete`預存程序。

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>步驟 5： 使用中的資料展示層

使用`EmployeesBLLWithSprocs`類別完成時，我們準備好可使用透過 ASP.NET 網頁的員工資料。 開啟`JOINs.aspx`頁面中`AdvancedDAL`資料夾，然後拖曳的 GridView，從 [工具箱] 拖曳至設計工具，設定其`ID`屬性設`Employees`。 接下來，從 [GridView] s 智慧標籤，將方格繫結至新的 ObjectDataSource 控制項，名為`EmployeesDataSource`。

設定要使用 ObjectDataSource`EmployeesBLLWithSprocs`類別，並從 選取和刪除索引標籤，確定`GetEmployees`和`DeleteEmployee`方法會從下拉式清單中選取。 按一下 [完成] 以完成 ObjectDataSource 的組態。


[![設定使用 EmployeesBLLWithSprocs 類別 ObjectDataSource](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**圖 12**： 設定要使用 ObjectDataSource`EmployeesBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![具有 ObjectDataSource 使用 GetEmployees 和 DeleteEmployee 方法](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**圖 13**: ObjectDataSource 使用了`GetEmployees`並`DeleteEmployee`方法 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio 會加入至 GridView BoundField 的每個`EmployeesDataTable`s 資料行。 移除所有除了這些 BoundFields `Title`， `LastName`， `FirstName`， `ManagerFirstName`，並`ManagerLastName`並重新命名`HeaderText`姓氏、 名字、 Manager s 第一個名稱，最後四個 BoundFields 屬性和管理員 s 姓氏，分別。

若要允許從這個頁面刪除員工的使用者，我們需要做兩件事。 首先，指示 GridView，以檢查它的智慧標籤的 啟用刪除選項提供刪除的功能。 第二，變更 ObjectDataSource s `OldValuesParameterFormatString` ObjectDataSource 精靈設定屬性的值 (`original_{0}`) 為其預設值 (`{0}`)。 進行這些變更之後，您的 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

造訪透過瀏覽器來測試頁面。 如 [圖 14] 所示，此頁面會列出每一位員工和 （假設有的話） 的他或她 manager s 名稱。


[![Employees_Select 參與預存程序會傳回經理的名稱](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**圖 14**:`JOIN`中`Employees_Select`預存程序傳回的管理員名稱 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


按一下 [刪除] 按鈕會啟動刪除最後的執行中的工作流程，`Employees_Delete`預存程序。 不過，嘗試`DELETE`預存程序的陳述式會失敗，因為外部索引鍵條件約束違規情形 （請參閱 圖 15）。 具體來說，每一位員工會有一或多個記錄`Orders`資料表中，導致無法刪除。


[![刪除某位員工具有對應的訂單結果中外部索引鍵條件約束違規](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**圖 15**： 刪除某位員工具有對應的訂單結果中外部索引鍵條件約束違規 ([按一下以檢視完整大小的影像](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


若要讓員工可刪除您可以：

- 更新外部索引鍵條件約束的串聯刪除
- 以手動方式刪除資料錄`Orders`employee(s) 您想要刪除的資料表或
- 更新`Employees_Delete`預存程序，先刪除相關的記錄，從`Orders`資料表，然後再刪除`Employees`記錄。 我們會討論此技巧[使用現有預存程序的輸入資料集 Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程。

我不要更動此練習的讀取器。

## <a name="summary"></a>總結

使用關聯式資料庫時, 很常見的查詢，以提取其資料從多個相關資料表。 相互關聯子查詢和`JOIN`提供兩種不同技術來存取資料，從查詢中的相關資料表。 在我們最常做的上一個教學課程使用的相互關聯子查詢，因為 TableAdapter 無法自動產生`INSERT`， `UPDATE`，並`DELETE`陳述式中的查詢涉及`JOIN`s。 雖然使用特定 SQL 陳述式完成 [TableAdapter 組態精靈] 之後的任何自訂將會覆寫時，可以手動提供這些值。

幸運的是，使用預存程序所建立的 Tableadapter 不會發生與使用特定 SQL 陳述式所建立之相同脆弱度。 因此，就可以建立其主要的查詢會使用的 TableAdapter`JOIN`時使用預存程序。 在本教學課程中，我們了解如何建立這類的 TableAdapter 的內容。 我們開始使用`JOIN`-較少`SELECT`TableAdapter s 主查詢的查詢，以便對應的 insert、 update 和 delete 預存程序會自動建立。 TableAdapter s 初始設定完成後，我們擴大`SelectCommand`預存程序，使用`JOIN`並重新執行 TableAdapter 組態精靈以更新`EmployeesDataTable`s 資料行。

重新執行 TableAdapter 組態精靈會自動更新`EmployeesDataTable`以反映所傳回的資料欄位的資料行`Employees_Select`預存程序。 或者，我們可能有這些資料行手動新增至 DataTable。 我們將探討以手動方式新增的資料行至 DataTable，在下一個教學課程。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Geisenow、 David Suru 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [下一頁](adding-additional-datatable-columns-cs.md)
