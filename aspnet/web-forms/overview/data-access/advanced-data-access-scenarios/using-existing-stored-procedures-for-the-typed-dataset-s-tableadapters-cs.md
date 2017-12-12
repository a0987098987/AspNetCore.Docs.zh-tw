---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: "使用現有的預存程序的具類型資料集的 Tableadapter (C#) |Microsoft 文件"
author: rick-anderson
description: "在上一個教學課程中我們學到如何使用 TableAdapter 精靈來產生新的預存程序。 在此教學課程中我們了解如何在相同 TableAdapter..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b76f0ac07051496c6f34be41dcf20154e34674
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>使用現有的預存程序的具類型資料集的 Tableadapter (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip)或[下載 PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> 在上一個教學課程中我們學到如何使用 TableAdapter 精靈來產生新的預存程序。 本教學課程中我們了解如何相同 TableAdapter 精靈可搭配現有的預存程序。 我們也會了解如何以手動方式將新的預存程序加入至我們的資料庫。


## <a name="introduction"></a>簡介

在[前述教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)我們可了解如何 s Tableadapter 的型別資料集無法設定為使用預存程序存取資料，而不是特定 SQL 陳述式。 特別是，我們檢驗如何以自動建立這些預存程序 [Tabaleadapter 精靈]。 當舊版應用程式移植到 ASP.NET 2.0 或建置現有的資料模型周圍 ASP.NET 2.0 網站，有可能是資料庫已包含我們需要的預存程序。 或者，您可能想要建立預存程序，以手動方式或透過您的預存程序會自動產生 [Tabaleadapter 精靈] 以外的某些工具。

在此教學課程中我們將探討如何設定要使用現有的預存程序的 TableAdapter。 因為 Northwind 資料庫只會有較少的內建的預存程序，我們也會探討手動將新的預存程序新增至 Visual Studio 環境透過資料庫所需的步驟。 Let s 開始 ！

> [!NOTE]
> 在[包裝在交易內的資料庫修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)教學課程中我們將方法加入 TableAdapter 為支援交易 (`BeginTransaction`，`CommitTransaction`等等)。 或者，可以完全在預存程序，不需要修改的資料存取層程式碼來管理交易。 在本教學課程中，我們將探討用來執行預存程序 s 陳述式的範圍內的交易的 T-SQL 命令。


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>步驟 1： 將預存程序加入至 Northwind 資料庫

Visual Studio 輕鬆地將新的預存程序加入至資料庫。 O d e s 傳回所有資料行，從 Northwind 資料庫中加入新的預存程序`Products`適用於具有特定資料表`CategoryID`值。 從 [伺服器總管] 視窗中展開 Northwind 資料庫，讓其資料夾-資料庫圖表、 資料表、 檢視和等位會顯示。 如我們所見前述教學課程中，這個預存程序資料夾包含的資料庫 s 現有預存程序。 若要加入新的預存程序，只要以滑鼠右鍵按一下 預存程序 資料夾，並從內容功能表中選擇 加入新的預存程序選項。


[![以滑鼠右鍵按一下 [預存程序] 資料夾，並加入新的預存程序](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**圖 1**： 以滑鼠右鍵按一下預存程序 」 資料夾，並加入新的預存程序 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


如圖 1 所示，選取 [加入新的預存程序] 選項會開啟的指令碼視窗 Visual Studio 中與建立預存程序所需的 SQL 指令碼的大綱。 就是我們來充實此指令碼和執行它，此時預存程序將會加入至資料庫。

輸入下列指令碼：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

此指令碼，在執行時，會將新的預存程序加入至名為 Northwind 資料庫`Products_SelectByCategoryID`。 這個預存程序會接受單一輸入的參數 (`@CategoryID`，型別`int`)，它會傳回所有符合這些產品欄位`CategoryID`值。

若要執行此`CREATE PROCEDURE`指令碼和預存程序加入至資料庫、 按一下工具列中的 [儲存] 圖示或按 Ctrl + S。 這樣做之後，預存程序資料夾重新整理，顯示新建立預存程序。 此外，在視窗中的指令碼將變更從微妙的地方`CREATE PROCEDURE dbo.Products_SelectProductByCategoryID`至`ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`。 `CREATE PROCEDURE`在資料庫中，加入新的預存程序時`ALTER PROCEDURE`更新現有。 因為指令碼開頭已變更為`ALTER PROCEDURE`，變更預存程序輸入參數或 SQL 陳述式，並按一下 [儲存] 圖示將會使用這些變更更新預存程序。

圖 2 顯示 Visual Studio 之後`Products_SelectByCategoryID`尚未儲存預存程序。


[![預存程序 Products_SelectByCategoryID 加入資料庫](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**圖 2**: 預存程序`Products_SelectByCategoryID`已加入至資料庫 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>步驟 2： 設定要使用現有的預存程序的 TableAdapter

既然`Products_SelectByCategoryID`預存程序已加入至資料庫，我們可以 concigure 我們的資料存取層，其中一個方法叫用時，請使用此預存程序。 特別是，我們將會加入`GetProducstByCategoryID(categoryID)`方法`ProductsTableAdapter`中`NorthwindWithSprocs`呼叫的型別資料集`Products_SelectByCategoryID`預存程序剛才所建立。

先開啟`NorthwindWithSprocs`資料集。 以滑鼠右鍵按一下`ProductsTableAdapter`，然後選擇 加入查詢，啟動 TableAdapter 查詢組態精靈。 在[前述教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)我們選擇已建立新的預存程序為我們的 TableAdapter。 此教學課程中，不過，我們想要連接至現有的新的 TableAdapter 方法`Products_SelectByCategoryID`預存程序。 因此，精靈 s 第一個步驟中選擇使用現有的預存程序選項，然後按一下 [下一步]。


[![選擇 使用現有的預存程序選項](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**圖 3**： 選擇使用現有的預存程序選項 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


下列畫面會提供下拉式清單填入資料庫 s 預存程序。 選取預存程序會列出其左邊和右邊傳回 （如果有的話） 的資料欄位的輸入的參數。 選擇`Products_SelectByCategoryID`預存程序，從清單中，按一下 [下一步]。


[![挑選 Products_SelectByCategoryID 預存程序](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**圖 4**： 挑選`Products_SelectByCategoryID`預存程序 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


下一個畫面會詢問您的資料類型由預存程序，我們在這裡的回答決定了 TableAdapter 的方法所傳回的類型。 例如，如果我們表示會傳回表格式資料，則方法會傳回`ProductsDataTable`填入傳回記錄的預存程序的執行個體。 相反地，如果我們指出此預存程序傳回單一值會傳回 TableAdapter`object`指派預存程序所傳回的第一個記錄的第一個資料行中的值。

因為`Products_SelectByCategoryID`預存程序會傳回屬於特定類別，選擇第一個回應的表格式資料-然後按下一步的所有產品。


[![指出預存程序傳回表格式資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**圖 5**： 指出預存程序傳回表格式資料 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


剩下的就是以指出方法模式來使用這些方法的名稱後面。 保留的填滿 DataTable 和 傳回 DataTable 選項選取此選項，但重新命名的方法`FillByCategoryID`和`GetProductsByCategoryID`。 然後按一下 [下一步] 檢閱精靈即將執行的工作的摘要。 如果一切正確，按一下 [完成]。


[![名稱方法 FillByCategoryID 和 GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**圖 6**: Name 方法`FillByCategoryID`和`GetProductsByCategoryID`([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> 我們剛建立的 TableAdapter 方法`FillByCategoryID`和`GetProductsByCategoryID`，預期類型的輸入的參數`int`。 此輸入的參數值傳遞至預存程序，透過其`@CategoryID`參數。 如果您修改`Products_SelectByCategory`預存程序的參數，您必須也更新這些 TableAdapter 方法的參數。 中所述[上一個教學課程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)，作法是在兩種方式之一： 透過手動新增或移除參數從參數集合，或返回 [Tabaleadapter 精靈]。


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>步驟 3： 加入`GetProductsByCategoryID(categoryID)`BLL 方法

與`GetProductsByCategoryID`DAL 方法完成下, 一個步驟是讓商務邏輯層中，這個方法的存取。 開啟`ProductsBLLWithSprocs`類別檔案，然後將下列方法：


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

這個 BLL 方法只會傳回`ProductsDataTable`從傳回`ProductsTableAdapter`s`GetProductsByCategoryID`方法。 `DataObjectMethodAttribute`屬性提供 ObjectDataSource s 設定資料來源精靈 所使用的中繼資料。 特別是，這個方法會出現在選取的索引標籤的下拉式清單。

## <a name="step-4-displaying-products-by-category"></a>步驟 4： 顯示產品類別目錄

若要測試新加入`Products_SelectByCategoryID`預存程序和對應的 DAL 和 BLL 方法，讓建立 ASP.NET 網頁，其中包含 DropDownList 和 GridView。 DropDownList 會列出所有資料庫中的類別時 GridView 會顯示屬於所選類別目錄的產品。

> [!NOTE]
> 我們已建立主從式介面在先前的教學課程中使用 DropDownLists。 更深入探討實作主要/詳細資料報表，請參閱[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程。


開啟`ExistingSprocs.aspx`頁面`AdvancedDAL`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 DropDownList。 設定 DropDownList s`ID`屬性`Categories`及其`AutoPostBack`屬性`true`。 接下來，從其智慧標籤，將繫結 DropDownList 至名為新 ObjectDataSource `CategoriesDataSource`。 因此，它會擷取資料的來源設定 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法。 設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![從 CategoriesBLL 類別的 GetCategories 方法擷取資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**圖 7**： 擷取資料，從`CategoriesBLL`類別 s`GetCategories`方法 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**圖 8**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


完成 ObjectDataSource 精靈之後，設定要顯示 DropDownList`CategoryName`資料欄位並使用`CategoryID`欄位做為`Value`每個`ListItem`。

此時，DropDownList 和 ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

下一步，拖曳至設計工具中，將此硬碟置於 DropDownList 下方的 GridView。 設定 GridView s`ID`至`ProductsByCategory`和其智慧標籤上，從繫結到名為新 ObjectDataSource `ProductsByCategoryDataSource`。 設定`ProductsByCategoryDataSource`使用 ObjectDataSource`ProductsBLLWithSprocs`類別，它會擷取其資料使用`GetProductsByCategoryID(categoryID)`方法。 此 GridView 將只可用來顯示資料，因為下拉式清單中更新、 插入和刪除索引標籤為 （無） 並按下一步。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**圖 9**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![從 GetProductsByCategoryID(categoryID) 方法擷取資料](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**圖 10**： 擷取資料，從`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


在 [選取] 索引標籤中選擇的方法預期參數，因此精靈的最後一個步驟會將我們提示參數 s 的來源。 設定參數來源下拉式清單控制項，然後選擇 `Categories`控制項從 ControlID 下拉式清單。 按一下 [完成] 5d; 以完成精靈。


[![使用類別 DropDownList 做 categoryID 參數的來源](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**圖 11**： 使用`Categories`做為來源的 DropDownList`categoryID`參數 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


完成 [ObjectDataSource] 精靈，Visual Studio 會加入 BoundFields 以及 CheckBoxField 每個產品資料欄位。 請隨意自訂這些欄位，根據您的需要。

瀏覽透過瀏覽器的頁面。 當瀏覽 [已選取的飲料類別目錄] 頁面和對應方格中列出的產品。 將下拉式清單變更為替代的類別，圖 12 顯示、 導致回傳並重新載入格線，內含新選取的類別目錄的產品。


[![會顯示產生的類別目錄中的產品](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**圖 12**： 會顯示產生的類別目錄中的產品 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>步驟 5： 包裝預存程序 s 陳述式的交易範圍內

在[包裝在交易內的資料庫修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)教學課程中我們將討論資料庫修改陳述式的範圍內的交易的一系列的技術。 提醒您，或是執行起交易的修改所有成功 」 或 「 所有的失敗，保證不可部份完成性。 使用的交易的相關技巧包括：

- 使用中的類別`System.Transactions`命名空間，
- 讓資料存取層使用 ADO.NET 類別，例如`SqlTransaction`，和
- 加入直接在預存程序內的 T-SQL 和交易命令

*包裝在交易內的資料庫修改*教學課程中 DAL 使用 ADO.NET 類別。 本教學課程的其餘部分會檢查如何管理使用 T-SQL 命令從預存程序內的交易。

手動啟動、 認可，以及回復交易的三個主要 SQL 命令是`BEGIN TRANSACTION`， `COMMIT TRANSACTION`，和`ROLLBACK TRANSACTION`分別。 使用預存程序，我們需要套用下列模式中的交易時的 ADO.NET 方法，例如：

1. 表示開始的交易。
2. 執行 SQL 陳述式組成的交易。
3. 如果任何一種從步驟 2 中，復原交易的陳述式錯誤
4. 如果所有的步驟 2 中的陳述式完成不會發生錯誤時，會認可交易。

可以實作此模式中使用下列範本的 T-SQL 語法：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

範本會定義啟動`TRY...CATCH`區塊中，新增到 SQL Server 2005 的建構。 例如`try...catch`封鎖在 C# 中，SQL`TRY...CATCH`區塊會執行中的陳述式`TRY`區塊。 如果任何陳述式引發錯誤，控制權會立即轉移到`CATCH`區塊。

如果沒有執行 SQL 陳述式的交易，該結構的錯誤`COMMIT TRANSACTION`陳述式認可的變更並完成交易。 如果其中一個陳述式導致錯誤，不過，`ROLLBACK TRANSACTION`中`CATCH`區塊將使資料庫回到其交易啟動之前的狀態。 預存程序也會引發錯誤使用[RAISERROR 命令](https://msdn.microsoft.com/en-us/library/ms178592.aspx)，這會導致`SqlException`在應用程式中引發。

> [!NOTE]
> 因為`TRY...CATCH`區塊的新 SQL Server 2005、 如果您使用舊版的 Microsoft SQL Server，將無法使用上述的範本。 如果您不使用 SQL Server 2005，請參閱[管理 SQL Server 預存程序中的交易](http://www.4guysfromrolla.com/webtech/080305-1.shtml)的範本，將會使用其他版本的 SQL Server。


可讓 s 查看具體的範例。 之間存在的外部索引鍵條件約束`Categories`和`Products`資料表，這表示，每個`CategoryID`欄位`Products`資料表必須對應到`CategoryID`值`Categories`資料表。 任何違反這個條件約束，例如嘗試刪除具有相關聯的產品類別目錄的動作會導致外部索引鍵條件約束違規。 若要確認這種情況，再次瀏覽正在更新及刪除現有的二進位資料的範例中使用二進位資料區段 (`~/BinaryData/UpdatingAndDeleting.aspx`)。 此頁面會列出每個類別目錄中編輯和刪除按鈕 （請參閱圖 13），以及系統，但刪除如果您嘗試刪除具有相關聯的產品，例如飲料-類別卻因外部索引鍵條件約束違規而失敗 （請參閱圖 14）。


[![每個類別都會顯示於編輯和刪除按鈕的 GridView](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**圖 13**： 每個類別都會顯示於編輯和刪除按鈕的 GridView ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![您無法刪除具有現有的產品類別目錄](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**圖 14**： 您無法刪除具有現有的產品類別目錄 ([按一下以檢視完整大小的影像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


不過，假設我們想要允許刪除不論它們是否有關聯的產品類別目錄。 會刪除與產品類別目錄，假設我們想要同時刪除其現有的產品 (雖然另一個選項，可直接將其產品`CategoryID`值`NULL`)。 這項功能可能會透過外部索引鍵條件約束的 cascade 規則實作。 或者，我們可以建立可接受的預存程序`@CategoryID`輸入的參數並叫用時，明確地刪除所有相關聯的產品，然後指定的分類。

我們第一次嘗試在這樣的預存程序可能如下所示：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

這肯定會刪除相關聯的產品和分類，而它不會這樣起的交易。 假設上是一些其他外部索引鍵條件約束`Categories`，會禁止刪除特定`@CategoryID`值。 問題是，在這種情況下的所有產品會刪除之前，我們嘗試刪除類別目錄。 最後結果就是，這類的類別目錄，此預存程序會移除所有的產品類別目錄變得讚賞，因為它仍有相關的其他資料表中的記錄時。

如果預存程序包裝範圍內的交易，不過，若要刪除`Products`資料表會面臨無法刪除上回復`Categories`。 下列預存程序指令碼會使用交易，以確保兩者之間的不可部份完成性`DELETE`陳述式：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

請花一點時間加入`Categories_Delete`預存程序至 Northwind 資料庫。 步驟 1 回頭參考，如需將預存程序加入資料庫中的指示。

## <a name="step-6-updating-thecategoriestableadapter"></a>步驟 6： 更新`CategoriesTableAdapter`

我們已加入`Categories_Delete`預存程序至資料庫，DAL 目前設定為使用特定 SQL 陳述式來執行刪除。 我們需要更新`CategoriesTableAdapter`並指示它使用`Categories_Delete`改為預存程序。

> [!NOTE]
> 稍早在本教學課程中我們使用`NorthwindWithSprocs`資料集。 但該資料集只有單一實體， `ProductsDataTable`，因此需要使用 類別目錄。 因此，對於當我談論的資料存取層 I m 參考本教學課程的其餘部分`Northwind`資料集，我們在第一次建立一個[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程。


開啟 Northwind 資料集選取`CategoriesTableAdapter`，並移至 [屬性] 視窗。 屬性視窗清單`InsertCommand`， `UpdateCommand`， `DeleteCommand`，和`SelectCommand`使用 TableAdapter，因為其名稱和連接資訊。 展開`DeleteCommand`屬性，以查看其詳細資料。 如圖 15 所示， `DeleteCommand` s`ComamndType`屬性設定為指示它傳送文字的文字`CommandText`屬性做為特定 SQL 查詢。


![CategoriesTableAdapter 設計工具中選取 [屬性] 視窗中檢視其屬性](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**圖 15**： 選取`CategoriesTableAdapter`在設計工具中 [屬性] 視窗中檢視其屬性


若要變更這些設定，請在 屬性 視窗中選取 (DeleteCommand) 文字並從下拉式清單中選擇 （新增）。 這會清除的設定`CommandText`， `CommandType`，和`Parameters`屬性。 接下來，設定`CommandType`屬性`StoredProcedure`，請輸入名稱的預存程序和`CommandText`(`dbo.Categories_Delete`)。 如果您務必依此順序中的輸入屬性，第一次`CommandType`然後`CommandText`-Visual Studio 會自動填入參數集合。 如果您沒有在此順序中輸入這些屬性，您必須手動加入參數的參數集合編輯器。 在任一情況下，它 s 審慎的作法是按一下省略符號，即可確認，正確的參數設定已變更 （請參閱圖 16） 的參數集合編輯器的 [參數] 屬性。 如果看不到任何參數，在對話方塊中，加入`@CategoryID`參數以手動方式 (您不需要加入`@RETURN_VALUE`參數)。


![確認參數設定正確](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**圖 16**： 確認參數設定是否正確


一旦更新 DAL 刪除類別目錄會自動刪除所有相關聯的產品並散布概括性的交易。 若要確認這種情況，返回 更新和刪除現有的二進位資料頁面上，按一下的其中一個類別目錄的 刪除 按鈕。 與一個單一按一下滑鼠，就會刪除類別目錄和所有相關聯的產品。

> [!NOTE]
> 在測試之前先`Categories_Delete`預存程序，將會刪除與所選類別目錄的產品數目，可能必須在製作資料庫的備份複本。 如果您使用`NORTHWND.MDF`資料庫`App_Data`，只需關閉 Visual Studio，並將複製的 MDF 和 LDF 檔案中`App_Data`為其他資料夾。 之後測試的功能，您可以關閉 Visual Studio 來還原資料庫，並取代目前的 MDF 和 LDF 檔案`App_Data`以備份複本。


## <a name="summary"></a>總結

雖然我們，TableAdapter 的精靈會自動產生預存程序，但有些的時候，當我們可能已經有這類預存程序，建立或想要它們以手動方式或使用其他工具改為建立。 為了配合這種情況下，TableAdapter 也可以設定為指向現有的預存程序。 本教學課程中我們會討論如何手動新增到資料庫中透過 Visual Studio 環境的 預存程序，以及如何連接 TableAdapter 的方法，這些預存程序。 此外，我們也會檢查 T-SQL 命令和指令碼模式，用於啟動、 認可，以及回復預存程序內的交易。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Hilton Geisenow、 S ren 因此 Lauritsen 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
[下一頁](updating-the-tableadapter-to-use-joins-cs.md)
