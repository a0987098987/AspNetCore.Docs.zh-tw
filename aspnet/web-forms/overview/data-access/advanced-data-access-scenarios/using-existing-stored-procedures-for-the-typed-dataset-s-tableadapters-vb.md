---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: 使用現有的預存程序為具類型資料集的 Tableadapter (VB) |Microsoft Docs
author: rick-anderson
description: 在上一個教學課程中，我們了解如何使用 TableAdapter 精靈 產生新的預存程序的內容。 在本教學課程中我們了解如何在相同 TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a478b52387a6bbf726872978a0ad4a83c22c9911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382892"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>使用現有的預存程序為具類型資料集的 Tableadapter (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip)或[下載 PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> 在上一個教學課程中，我們了解如何使用 TableAdapter 精靈 產生新的預存程序的內容。 在本教學課程中我們了解如何相同 TableAdapter 精靈 可以搭配現有的預存程序。 我們也了解如何以手動方式將新的預存程序新增至我們的資料庫。


## <a name="introduction"></a>簡介

在 [前述教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)我們可了解如何 s TableAdapters 的具類型資料集無法設定為使用預存程序，以存取資料，而不是特定 SQL 陳述式。 特別是，我們會檢查如何讓 TableAdapter 精靈會自動建立這些預存程序。 移植到 ASP.NET 2.0 的舊版應用程式時，或建置現有的資料模型周圍的 ASP.NET 2.0 網站時，有可能是資料庫已包含我們所需要的預存程序。 或者，您也可能會想要建立您的預存程序，以手動方式或透過您的預存程序會自動產生 [Tabaleadapter 精靈] 以外的一些工具。

在本教學課程中我們將探討如何設定要使用現有的預存程序的 TableAdapter。 因為 Northwind 資料庫只有較少的內建的預存程序，我們也將探討手動透過 Visual Studio 環境的資料庫中加入新的預存程序所需的步驟。 讓 s 開始 ！

> [!NOTE]
> 在 [資料庫修改包裝在交易](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程中我們加入來支援交易的 TableAdapter 方法 (`BeginTransaction`，`CommitTransaction`等等)。 或者，可以完全在不需要對資料存取層的程式碼進行任何修改的預存程序內管理交易。 在本教學課程中，我們將探討用來執行預存程序 s 陳述式的範圍內的交易的 T-SQL 命令。


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>步驟 1： 加入 Northwind 資料庫中的預存程序

Visual Studio 可讓您輕鬆地將新的預存程序加入至資料庫。 可讓 s 傳回所有資料行，從 Northwind 資料庫中加入新的預存程序`Products`適用於具有特定資料表`CategoryID`值。 從 [伺服器總管] 視窗中，展開 Northwind 資料庫，使其-資料庫圖表、 資料表、 檢視和等位的資料夾會顯示。 如我們所見在先前的教學課程中，預存程序 資料夾會包含資料庫 s 現有預存程序。 若要加入新的預存程序，只要以滑鼠右鍵按一下 預存程序 資料夾，並從內容功能表中選擇 加入新的預存程序選項。


[![以滑鼠右鍵按一下 [預存程序] 資料夾，並加入新的預存程序](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**圖 1**： 以滑鼠右鍵按一下 [預存程序] 資料夾，並加入新的預存程序 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


如 [圖 1] 所示，選取 [加入新的預存程序] 選項會開啟指令碼視窗在 Visual Studio 中建立預存程序所需的 SQL 指令碼的外框。 它是我們的責任充實這段指令碼，並執行它，此時預存程序會加入至資料庫。

輸入下列指令碼：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

此指令碼，在執行時，會將新的預存程序新增至名為 Northwind 資料庫`Products_SelectByCategoryID`。 這個預存程序會接受單一輸入的參數 (`@CategoryID`，型別的`int`)，則會傳回所有具有相符產品欄位`CategoryID`值。

若要執行這個`CREATE PROCEDURE`指令碼和預存程序新增至資料庫、 按一下工具列中的 [儲存] 圖示或按 Ctrl + S。 這麼做之後，預存程序的資料夾重新整理，顯示新建立預存程序。 此外，在視窗中的指令碼將變更從一些微妙的差異`CREATE PROCEDURE dbo.Products_SelectProductByCategoryID`要`ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`。 `CREATE PROCEDURE` 在資料庫中，加入新的預存程序雖然`ALTER PROCEDURE`更新現有的。 因為指令碼的開頭已變更為`ALTER PROCEDURE`，變更預存程序的輸入參數或 SQL 陳述式，並按一下 [儲存] 圖示將會使用這些變更更新預存程序。

[圖 2] 顯示之後的 Visual Studio`Products_SelectByCategoryID`已儲存預存程序。


[![預存程序 Products_SelectByCategoryID 加入資料庫](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**圖 2**: 預存程序`Products_SelectByCategoryID`已加入至資料庫 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>步驟 2： 設定要使用現有的預存程序的 TableAdapter

既然`Products_SelectByCategoryID`預存程序已新增至資料庫，我們可以設定我們的資料存取層，其中一個方法叫用時，請使用此預存程序。 特別是，我們會新增`GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->`方法，以`ProductsTableAdapter`中`NorthwindWithSprocs`呼叫的輸入資料集`Products_SelectByCategoryID`預存程序，我們剛剛建立。

首先開啟`NorthwindWithSprocs`資料集。 以滑鼠右鍵按一下`ProductsTableAdapter`，然後選擇 [加入查詢]，啟動 [TableAdapter 查詢組態精靈]。 在 [前述教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)我們選擇能夠為我們建立新的預存程序的 TableAdapter。 本教學課程中，不過，我們想要將新的 TableAdapter 方法傳送至現有`Products_SelectByCategoryID`預存程序。 因此，從精靈 s 第一個步驟中選擇 使用現有的預存程序選項，然後按一下 下一步。


[![選擇 使用現有的預存程序選項](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**圖 3**： 選擇 使用現有預存程序選項 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


下列畫面會提供下拉式清單中填入資料庫 s 預存程序。 選取預存程序會列出其左邊和右邊傳回 （如果有的話） 的資料欄位的輸入的參數。 選擇`Products_SelectByCategoryID`預存程序，從清單中，按一下 [下一步]。


[![挑選 Products_SelectByCategoryID 預存程序](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**圖 4**： 挑選`Products_SelectByCategoryID`預存程序 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


下一個畫面詢問我們的資料類型由預存程序，我們的解答決定 TableAdapter 的方法所傳回的型別。 例如，如果我們指出會傳回表格式資料，則方法會傳回`ProductsDataTable`填入預存程序所傳回之記錄的執行個體。 相反地，如果我們指出此預存程序傳回單一值會傳回 TableAdapter`Object`指派預存程序所傳回的第一筆記錄的第一個資料行中的值。

因為`Products_SelectByCategoryID`預存程序會傳回所有產品都屬於特定類別，選擇第一個回應-表格式資料集，按一下 [下一步]。


[![指出預存程序會傳回表格式資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**圖 5**： 表示預存程序會傳回表格式資料 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


所有剩下就是指出哪些方法的模式，以使用後面接著這些方法的名稱。 DataTable 和傳回 DataTable 選項已核取，但重新命名的方法，讓這兩種填滿`FillByCategoryID`和`GetProductsByCategoryID`。 然後按一下 [下一步] 檢閱精靈即將執行工作的摘要。 如果一切看起來正確，按一下 [完成]。


[![名稱方法 FillByCategoryID 和 GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**圖 6**: Name 方法`FillByCategoryID`並`GetProductsByCategoryID`([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> 我們剛才建立的 TableAdapter 方法`FillByCategoryID`並`GetProductsByCategoryID`，預期類型的輸入的參數`Integer`。 此輸入的參數的值會傳遞至預存程序，透過其`@CategoryID`參數。 如果您修改`Products_SelectByCategory`預存程序的參數，您必須一併更新這些 TableAdapter 方法的參數。 中所述[先前的教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)，這可以在下列其中一種： 透過手動新增或移除參數從參數集合，或透過重新執行 [Tabaleadapter 精靈]。


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>步驟 3： 加入`GetProductsByCategoryID(categoryID)`BLL 方法

使用`GetProductsByCategoryID`DAL 方法完成下, 一個步驟是提供商業邏輯層中，這個方法的存取。 開啟`ProductsBLLWithSprocs`類別檔案，並新增下列方法：


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

此 BLL 方法只會傳回`ProductsDataTable`傳回從`ProductsTableAdapter`s`GetProductsByCategoryID`方法。 `DataObjectMethodAttribute`屬性提供 ObjectDataSource s 設定資料來源精靈 所使用的中繼資料。 特別的是，這個方法會出現在 [選取] 索引標籤的下拉式清單中。

## <a name="step-4-displaying-products-by-category"></a>步驟 4： 顯示產品類別目錄

若要測試新加入`Products_SelectByCategoryID`預存程序和對應的 DAL 和 BLL 方法，讓建立 ASP.NET 網頁，其中包含 DropDownList 和 GridView。 DropDownList 會列出所有資料庫中的類別，而 GridView 會顯示屬於所選分類的產品。

> [!NOTE]
> 我們已建立主從式介面在先前的教學課程中使用 dropdownlist 進行。 如需更深入了解實作這類主版/詳細報告，請參閱[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教學課程。


開啟`ExistingSprocs.aspx`頁面中`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具的 dropdownlist 進行拖曳。 設定 DropDownList s`ID`屬性，以`Categories`及其`AutoPostBack`屬性設`True`。 接下來，從它的智慧標籤，將繫結 DropDownList 至名為新 ObjectDataSource `CategoriesDataSource`。 因此，它會擷取資料的來源設定 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法。 設定下拉式清單中更新、 插入和刪除 （無） 索引標籤。


[![從 CategoriesBLL 類別的 GetCategories 方法擷取資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**圖 7**： 擷取的資料，從`CategoriesBLL`類別 s`GetCategories`方法 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**圖 8**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


完成 ObjectDataSource 精靈之後，設定要顯示 DropDownList`CategoryName`資料欄位，並將`CategoryID`欄位做為`Value`每個`ListItem`。

此時，DropDownList 與 ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

接下來，拖曳至設計工具中，將它放在 DropDownList 下方的 GridView。 設定 GridView s`ID`要`ProductsByCategory`和它的智慧標籤，從繫結至名為新 ObjectDataSource `ProductsByCategoryDataSource`。 設定`ProductsByCategoryDataSource`若要使用的 ObjectDataSource`ProductsBLLWithSprocs`類別，讓它可讓您擷取其資料使用`GetProductsByCategoryID(categoryID)`方法。 因為此 GridView 只會用來顯示資料，設定下拉式清單中更新、 插入和刪除 （無） 索引標籤以及按一下 下一步。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**圖 9**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![從 GetProductsByCategoryID(categoryID) 方法擷取資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**圖 10**： 擷取的資料，從`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


在 [選取] 索引標籤中選擇的方法會預期參數，讓精靈的最後一個步驟會提示我們輸入的參數 s 的來源。 設定參數來源下拉式清單控制項，然後選擇 `Categories`控制項從 ControlID 下拉式清單。 按一下 完成 以完成精靈。


[![使用分類 DropDownList categoryID 參數的來源](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**圖 11**： 使用`Categories`做為來源的 DropDownList`categoryID`參數 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


完成後 ObjectDataSource 精靈，Visual Studio 會將 BoundFields 及其每個產品的資料欄位。 放心地依照您自訂這些欄位。

瀏覽透過瀏覽器頁面。 當瀏覽所選取的飲料類別頁和對應方格中列出的產品。 將下拉式清單變更為替代的類別，以 圖 12 顯示、 造成回傳並重新載入新選取的類別目錄的產品與格線。


[![顯示產生的類別目錄中的產品](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**圖 12**： 會顯示產生的類別目錄中的產品 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>步驟 5： 包裝預存程序 s 陳述式的交易範圍內

在 [資料庫修改包裝在交易](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)教學課程中我們討論用來執行一系列資料庫修改陳述式的範圍內的交易的技巧。 修改交易的傘狀下執行的是所有成功的重新叫用 」 或 「 所有失敗，確保不可部分完成性。 使用交易的相關技巧包括：

- 使用中的類別`System.Transactions`命名空間，
- 讓資料存取層使用之類的 ADO.NET 類別`SqlTransaction`，及
- 新增直接在預存程序內的 T-SQL 交易命令

*資料庫修改包裝在交易*教學課程中的 DAL 使用 ADO.NET 類別。 本教學課程的其餘部分將探討如何管理使用 T-SQL 命令從預存程序內的交易。

三個索引鍵的 SQL 命令為手動啟動、 認可和回復交易`BEGIN TRANSACTION`， `COMMIT TRANSACTION`，和`ROLLBACK TRANSACTION`分別。 使用預存程序，我們需要套用下列模式中的交易時，使用 ADO.NET 方法時，例如：

1. 表示開始的交易。
2. 執行 SQL 陳述式構成交易。
3. 如果沒有任何一種從步驟 2 中，復原交易的陳述式錯誤。
4. 如果所有步驟 2 中的陳述式完成不會發生錯誤時，會認可交易。

在 T-SQL 語法中使用下列範本，您可以實作此模式：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

範本會定義啟動`TRY...CATCH`封鎖，SQL Server 2005 的新建構。 如同`Try...Catch`在 Visual Basic 中的 SQL 封鎖`TRY...CATCH`區塊都會執行中的陳述式`TRY`區塊。 如果任何陳述式會引發錯誤，控制權會立即轉移到`CATCH`區塊。

如果不沒有執行 SQL 陳述式的交易，該結構的任何錯誤`COMMIT TRANSACTION`陳述式認可變更，並完成交易。 不過，有一個陳述式會導致發生錯誤時，如果`ROLLBACK TRANSACTION`在`CATCH`區塊將使資料庫回到交易啟動之前的狀態。 預存程序也會引發錯誤，使用[RAISERROR 命令](https://msdn.microsoft.com/library/ms178592.aspx)，會造成`SqlException`在應用程式中引發。

> [!NOTE]
> 因為`TRY...CATCH`區塊的新 SQL Server 2005 中，如果您使用舊版的 Microsoft SQL Server，就不適用上述的範本。 如果您不使用 SQL Server 2005，請參閱[SQL Server 預存程序中的 管理交易](http://www.4guysfromrolla.com/webtech/080305-1.shtml)適用於 SQL Server 的其他版本的範本。


可讓 s 看看一個具體的範例。 之間存在的外部索引鍵條件約束`Categories`並`Products`資料表，這表示，每個`CategoryID`欄位中`Products`資料表必須對應至`CategoryID`中的值`Categories`資料表。 違反這個條件約束，例如嘗試刪除類別目錄相關聯的產品，任何動作都會導致外部索引鍵條件約束違規。 若要確認這種情況，重新瀏覽正在更新及刪除現有的二進位資料的範例中使用二進位資料 區段 (`~/BinaryData/UpdatingAndDeleting.aspx`)。 此頁面會列出每個類別，以及編輯和刪除按鈕 （請參閱 圖 13），系統中，但刪除如果您嘗試刪除相關聯的產品-例如飲料-類別目錄會失敗，因為外部索引鍵條件約束違規情形 （請參閱 圖 14）。


[![每個類別會顯示在 GridView 編輯和刪除按鈕](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**圖 13**： 每個類別會顯示在 GridView 編輯和刪除按鈕 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![您無法刪除有現有的產品類別目錄](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**圖 14**： 您無法刪除有現有的產品類別目錄 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


不過，假設我們想要允許刪除不論它們是否有相關的產品類別目錄。 若刪除與產品類別目錄的資訊，假設我們想要同時刪除其現有的產品 (雖然另一個選項是直接設定其產品`CategoryID`值來`NULL`)。 這項功能可能會實作透過外部索引鍵條件約束的串聯規則。 或者，我們可以在其中建立可接受的預存程序`@CategoryID`輸入的參數並叫用時，明確地刪除所有相關聯的產品，然後指定的分類。

我們在這種預存程序的第一次嘗試可能會如下所示：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

這肯定會刪除相關聯的產品和分類，而它不是在交易的保護傘。 Imagine 上的某些其他外部索引鍵條件約束`Categories`，則會禁止刪除特定`@CategoryID`值。 問題是，在此情況下的所有產品將會刪除之前我們嘗試刪除類別目錄。 最後結果就是，這類的類別目錄，此預存程序會移除所有的產品類別目錄保留，因為它仍有相關的其他資料表中的記錄時。

如果預存程序已包裝範圍內的交易，不過，若要刪除`Products`資料表會面臨無法刪除上回復`Categories`。 下列的預存程序指令碼會使用交易來確保兩者之間的不可部分完成性`DELETE`陳述式：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

請花一點時間加入`Categories_Delete`預存程序至 Northwind 資料庫。 步驟 1 到回頭參考，如需將預存程序加入資料庫中的指示。

## <a name="step-6-updating-thecategoriestableadapter"></a>步驟 6： 更新`CategoriesTableAdapter`

雖然我們已新增`Categories_Delete`到資料庫，DAL 的預存程序目前設定為使用特定 SQL 陳述式來執行刪除。 我們需要更新`CategoriesTableAdapter`，並指示它使用`Categories_Delete`改為預存程序。

> [!NOTE]
> 稍早在本教學課程中，我們已使用與`NorthwindWithSprocs`資料集。 但該資料集只能有單一的實體， `ProductsDataTable`，因此需要使用分類。 因此，對於當我談到的資料存取層 I m 參考本教學課程的其餘部分`Northwind`資料集，我們先建立中的另一個[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程。


開啟 Northwind 資料集選取`CategoriesTableAdapter`，並移至 [屬性] 視窗。 屬性視窗清單`InsertCommand`， `UpdateCommand`， `DeleteCommand`，和`SelectCommand`使用 TableAdapter，以及其名稱和連接資訊。 展開`DeleteCommand`屬性，以查看其詳細資料。 如 [圖 15] 所示， `DeleteCommand` s`ComamndType`屬性設定為會指示它傳送的文字的文字`CommandText`為特定 SQL 查詢的屬性。


![在 [屬性] 視窗中檢視其屬性設計工具中選取 CategoriesTableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**圖 15**： 選取`CategoriesTableAdapter`在設計工具中，在 屬性 視窗中檢視其內容


若要變更這些設定，請選取 (DeleteCommand) 文字屬性] 視窗中，從下拉式清單中選擇 [（新增）。 這會清除的設定`CommandText`， `CommandType`，和`Parameters`屬性。 接下來，設定`CommandType`屬性，以`StoredProcedure`，然後輸入名稱的預存程序`CommandText`(`dbo.Categories_Delete`)。 如果您要確定依此順序中的輸入屬性，第一次`CommandType`，然後`CommandText`-Visual Studio 會自動填入參數集合。 如果您未依此順序輸入這些屬性，您必須手動新增參數的參數集合編輯器。 在任一情況下，它 s 按一下以確認，正確的參數設定已變更 （請參閱 圖 16） 顯示的參數集合編輯器的 參數 屬性中的省略符號上的容錯度加倍。 如果看不到任何參數，在對話方塊中，新增`@CategoryID`參數以手動方式 (您不需要新增`@RETURN_VALUE`參數)。


![請確定參數設定正確](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**圖 16**： 確保參數設定正確


DAL 已更新之後，刪除類別目錄會自動刪除所有與其相關聯的產品並進行交易的保護傘。 若要確認，返回 [更新和刪除現有的二進位資料] 頁面並按一下 [刪除] 按鈕的其中一個類別。 單一按一下的滑鼠，將刪除類別目錄和所有與其相關聯的產品。

> [!NOTE]
> 在測試之前`Categories_Delete`預存程序，這會刪除許多產品，以及選取的類別，可能必須先備份您的資料庫。 如果您使用`NORTHWND.MDF`資料庫中`App_Data`，只需關閉 Visual Studio，並複製中的 MDF 和 LDF 檔案`App_Data`為其他資料夾。 之後測試的功能，您可以藉由關閉 Visual Studio 還原資料庫，並取代目前的 MDF 和 LDF 檔案`App_Data`使用備份副本。


## <a name="summary"></a>總結

雖然 TableAdapter s 精靈 會自動為我們產生預存程序，但是有些的時候，當我們可能已有建立這類預存程序或想要改為建立它們以手動方式或使用其他工具。 為了滿足這類案例，TableAdapter 也可以設定以指向現有的預存程序。 在本教學課程中我們探討了如何以手動方式將預存程序新增至透過 Visual Studio 環境的資料庫，以及如何將 TableAdapter 的方法傳送至這些預存程序。 此外，我們也會檢查的 T-SQL 命令和指令碼模式，用於啟動、 認可和回復預存程序內的交易。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Geisenow、 S ren Jacob Lauritsen 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [下一頁](updating-the-tableadapter-to-use-joins-vb.md)
