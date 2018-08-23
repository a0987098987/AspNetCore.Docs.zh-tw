---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: 新增其他 DataTable 資料行 (VB) |Microsoft Docs
author: rick-anderson
description: 使用 TableAdapter 精靈時建立的具類型資料集，對應的 DataTable 包含主要資料庫查詢所傳回的資料行。 但有...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b5a1e1d6beb00079d754601860d0c25bc8a23e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833886"
---
<a name="adding-additional-datatable-columns-vb"></a>新增其他 DataTable 資料行 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip)或[下載 PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> 使用 TableAdapter 精靈時建立的具類型資料集，對應的 DataTable 包含主要資料庫查詢所傳回的資料行。 但有些情況下，當 DataTable 需要包含其他資料行。 在本教學課程中我們了解當我們需要其他 DataTable 資料行時，為何建議預存程序。


## <a name="introduction"></a>簡介

將 TableAdapter 加入時輸入的資料集，會將對應的 DataTable s 結構描述取決 TableAdapter s 主查詢。 比方說，如果主要的查詢會傳回資料欄位*A*， *B*，並*C*，DataTable 會有三個對應的資料行命名為*A*，*B*，並*C*。除了其主要的查詢，TableAdapter 還可以包含其他傳回，可能是，根據一些參數的資料子集的查詢。 比方說，在除了要`ProductsTableAdapter`s 主查詢，傳回所有產品的相關資訊，它也包含像是方法`GetProductsByCategoryID(categoryID)`和`GetProductByProductID(productID)`，傳回提供的參數為基礎的特定產品資訊。

S DataTable 的結構描述已反映 TableAdapter s 主查詢的模型適用於所有 TableAdapter 的方法會傳回相同或較少的資料欄位，比主查詢中所指定。 如果 TableAdapter 方法需要傳回其他資料欄位，然後我們應該展開 DataTable s 結構描述據以。 在 [主要/詳細說明使用具有詳細資料 DataList 的主要記錄項目符號清單](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)教學課程中我們會新增方法以`CategoriesTableAdapter`傳回`CategoryID`， `CategoryName`，和`Description`中定義的資料欄位加上的主查詢`NumberOfProducts`，報告的每個類別目錄相關聯的產品數目的其他資料欄位。 我們以手動方式新增至新的資料行`CategoriesDataTable`才能擷取`NumberOfProducts`資料欄位值從這個新方法。

中所述[上傳檔案](../working-with-binary-files/uploading-files-vb.md)教學課程，但必須小心使用 TableAdapters，使用特定 SQL 陳述式，並具有其資料欄位不精確地符合主要查詢的方法。 如果重新執行 [TableAdapter 組態精靈，它會更新所有 TableAdapter 的方法，使其資料的欄位] 清單中選取符合主要查詢。 因此，使用自訂資料行清單的任何方法將會還原成主查詢 s 資料行清單，而不會傳回預期的資料。 使用預存程序時，就不會發生此問題。

在本教學課程中我們將探討如何擴充為包含其他資料行的 DataTable s 結構描述。 使用特定 SQL 陳述式時的 tableadapter 之脆弱度，因為在本教學課程中我們將使用預存程序。 請參閱[建立新預存程序的輸入資料集 Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)並[使用現有預存程序的輸入資料集 Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程，如需詳細資訊設定 TableAdapter 以使用預存程序。

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>步驟 1： 加入`PriceQuartile`資料行`ProductsDataTable`

在 *建立新預存程序的輸入資料集 Tableadapter*教學課程中我們建立名為具類型資料集`NorthwindWithSprocs`。 此資料集目前包含兩個 Datatable:`ProductsDataTable`和`EmployeesDataTable`。 `ProductsTableAdapter`具有下列三種方法：

- `GetProducts` -傳回所有記錄的主要查詢`Products`資料表
- `GetProductsByCategoryID(categoryID)` -傳回所有具有指定的產品*categoryID*。
- `GetProductByProductID(productID)` -傳回具有指定的特定產品*productID*。

所有主要的查詢和兩個額外的方法傳回的資料欄位，也就是所有的資料行相同集`Products`資料表。 有任何相互關聯子查詢或`JOIN`s 提取相關的資料，從`Categories`或`Suppliers`資料表。 因此，`ProductsDataTable`對應的資料行中每個欄位`Products`資料表。

本教學課程中，可讓 s 將方法加入`ProductsTableAdapter`名為`GetProductsWithPriceQuartile`傳回所有產品。 除了標準產品資料欄位中，`GetProductsWithPriceQuartile`也會包含`PriceQuartile`資料欄位，指出產品的價格落在哪一個四分位數。 比方說，這些產品的價格均以最耗費資源的 25%會有`PriceQuartile`值為 1，而這些價格落在下 25%會有的值為 4。 我們會擔心建立預存程序來傳回這項資訊之前，不過，我們必須先更新`ProductsDataTable`包含的資料行來保存`PriceQuartile`結果時`GetProductsWithPriceQuartile`方法可用。

開啟`NorthwindWithSprocs`資料集，以滑鼠右鍵按一下`ProductsDataTable`。 從操作功能表選擇 新增，然後選擇 資料行。


[![將新的資料行新增至 ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**圖 1**： 加入新的資料行，以`ProductsDataTable`([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image3.png))


這會將新的資料行新增至名為 Column1 型別的 DataTable `System.String`。 我們需要更新此資料行的名稱 PriceQuartile 和其類型`System.Int32`因為它會用來保存 1 到 4 之間的數字。 選取新加入的資料行，在`ProductsDataTable`，然後從 屬性 視窗中，設定`Name`PriceQuartile 的屬性和`DataType`屬性設`System.Int32`。


[![設定新的資料行的名稱和資料類型屬性](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**圖 2**： 設定新的資料行 s`Name`並`DataType`屬性 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image6.png))


如 [圖 2] 所示，有可能被設定，例如資料行的值是否必須是唯一的資料行是否自動遞增資料行的其他屬性，zda bude 資料庫`NULL`值允許，依此類推。 保留為其預設值設定這些值。

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>步驟 2： 建立`GetProductsWithPriceQuartile`方法

既然`ProductsDataTable`已更新為包含`PriceQuartile`資料行中，我們已準備好建立`GetProductsWithPriceQuartile`方法。 啟動 TableAdapter 上按一下滑鼠右鍵，然後從操作功能表選擇加入查詢。 這會顯示 TableAdapter 查詢組態精靈，它首先會提示我們輸入有關我們是否想要使用特定 SQL 陳述式或新的或現有的預存程序。 因為我們不您尚未建立傳回價格四分位數資料的預存程序，可讓 s 允許我們建立此預存程序的 TableAdapter。 選取 建立新的預存程序選項，然後按一下 下一步。


[![指示 TableAdapter 精靈為我們建立預存程序](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**圖 3**： 指示 TableAdapter 精靈以建立預存程序如我們 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image9.png))


在後續畫面中，[圖 4] 所示精靈會詢問我們要加入查詢的類型。 由於`GetProductsWithPriceQuartile`方法會傳回所有資料行和資料錄從`Products`資料表中，選取 選取會傳回資料列的選項，然後按 下一步。


[![我們的查詢將會在 SELECT 陳述式，傳回多個資料列](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**圖 4**： 我們的查詢將會`SELECT`陳述式，傳回多個資料列 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image12.png))


接下來我們會提示輸入`SELECT`查詢。 在精靈中輸入下列查詢：


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

上述查詢會使用 SQL Server 2005 s 新[`NTILE`函式](https://msdn.microsoft.com/library/ms175126.aspx)若要將結果分成四個群組的群組由決定`UnitPrice`以遞減順序排序的值。

不幸的是，查詢產生器不知道如何剖析`OVER`關鍵字和剖析上述查詢時，會顯示錯誤。 而不需使用查詢產生器，因此，直接在文字方塊中，在精靈中輸入上述的查詢。

> [!NOTE]
> 如需有關 NTILE 和 SQL Server 2005 s 其他排名函式，請參閱[傳回等級結果與 Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml)並[次序函數區段](https://msdn.microsoft.com/library/ms189798.aspx)從[SQLServer 2005 線上叢書 》](https://msdn.microsoft.com/library/ms189798.aspx)。


輸入後`SELECT`查詢，並按一下 [下一步]，精靈會要求我們提供的名稱，它會建立預存程序。 命名新的預存程序`Products_SelectWithPriceQuartile`，按一下 [下一步]。


[![命名預存程序 Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**圖 5**： 命名預存程序`Products_SelectWithPriceQuartile`([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image15.png))


最後，我們會提示您命名的 TableAdapter 方法。 保留的填滿 DataTable，然後傳回 DataTable 核取方塊已核取和名稱的方法`FillWithPriceQuartile`和`GetProductsWithPriceQuartile`。


[![名稱的 tableadapter 方法，然後按一下 [完成]](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**圖 6**： 名稱 TableAdapter 的方法並按一下 [完成] ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image18.png))


使用`SELECT`指定查詢和預存程序和 TableAdapter 方法名稱，按一下 [完成] 以完成精靈。 此時您可能會收到警告，或從精靈指出兩個`OVER`不支援 SQL 建構或陳述式。 您可以忽略這些警告。

完成精靈之後，應該包含 TableAdapter`FillWithPriceQuartile`並`GetProductsWithPriceQuartile`方法和資料庫應該包含名為預存程序`Products_SelectWithPriceQuartile`。 請花一點時間確認 TableAdapter 確實包含這個新方法，而且預存程序有已正確加入至資料庫。 如果您看不到預存程序再以滑鼠右鍵按一下 預存程序 資料夾並選擇 重新整理，請檢查資料庫。


![確認新的方法，已加入至 TableAdapter](adding-additional-datatable-columns-vb/_static/image19.png)

**圖 7**： 驗證新的方法，已加入至 TableAdapter


[![請確定資料庫包含 Products_SelectWithPriceQuartile 預存程序](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**圖 8**： 請確認資料庫包含`Products_SelectWithPriceQuartile`預存程序 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> 使用預存程序，而不特定 SQL 陳述式的優點之一是重新執行 TableAdapter 組態精靈將不會修改預存程序資料行清單。 驗證，TableAdapter 上按一下滑鼠右鍵，選擇 [設定] 選項，從內容功能表啟動精靈，然後按一下 [完成] 來完成它。 接下來，移至資料庫，然後檢視`Products_SelectWithPriceQuartile`預存程序。 請注意，不修改其資料行清單。 我們一直都使用特定 SQL 陳述式，重新執行 TableAdapter 組態精靈就會還原此查詢 s 資料行清單，以符合主要查詢資料行清單中，藉此將 NTILE 陳述式移除所使用的查詢`GetProductsWithPriceQuartile`方法。


當資料存取層 s`GetProductsWithPriceQuartile`叫用方法時，執行 TableAdapter`Products_SelectWithPriceQuartile`預存程序，並加入資料列來`ProductsDataTable`針對每個傳回資料錄。 預存程序所傳回的資料欄位會對應至`ProductsDataTable`s 資料行。 因為沒有`PriceQuartile`資料欄位所傳回的預存程序中，其值會指派給`ProductsDataTable`s`PriceQuartile`資料行。

其查詢不會傳回這些 TableAdapter 方法`PriceQuartile`資料欄位`PriceQuartile`資料行的值是所指定的值及其`DefaultValue`屬性。 如 [圖 2] 所示，此值設為`DBNull`，預設值。 如果您想要不同的預設值，只要將設定`DefaultValue`屬性據此。 只要確定`DefaultValue`值是有效的指定資料行 s `DataType` (亦即`System.Int32`如`PriceQuartile`資料行)。

此時，我們可以將額外的資料行加入至 DataTable 執行必要的步驟。 若要確認此額外的資料行如預期運作，讓建立 ASP.NET 網頁，顯示每個產品名稱、 價格和價格的四分位數。 這樣做之前，不過，我們必須先更新商務邏輯層，以納入方法呼叫向下到 DAL`GetProductsWithPriceQuartile`方法。 我們將在步驟 3 中，接下來，更新 BLL，然後再建立在步驟 4 中的 ASP.NET 網頁。

## <a name="step-3-augmenting-the-business-logic-layer"></a>步驟 3： 擴充商業邏輯層

我們使用新之前`GetProductsWithPriceQuartile`方法從展示層中，我們應該先將對應的方法加入 BLL。 開啟`ProductsBLLWithSprocs`類別檔案，然後將下列程式碼：


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

中的其他資料擷取方法一樣`ProductsBLLWithSprocs`，則`GetProductsWithPriceQuartile`方法會直接呼叫 DAL s 對應`GetProductsWithPriceQuartile`方法，並傳回其結果。

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>步驟 4： 在 ASP.NET 網頁中顯示的價格四分位數資訊

加上 BLL 完成我們準備好要建立的 ASP.NET 網頁，顯示每項產品的價格四分位數。 開啟`AddingColumns.aspx`頁面中`AdvancedDAL`資料夾，然後拖曳的 GridView，從 [工具箱] 拖曳至設計工具，設定其`ID`屬性設`Products`。 從 GridView s 智慧標籤，將它繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別的`GetProductsWithPriceQuartile`方法。 因為這會是唯讀的方格，設定下拉式清單中更新、 插入和刪除 （無） 索引標籤。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**圖 9**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image25.png))


[![從 GetProductsWithPriceQuartile 方法擷取產品資訊](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**圖 10**： 從擷取產品資訊`GetProductsWithPriceQuartile`方法 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image28.png))


完成設定資料來源精靈之後，Visual Studio 會自動加入 BoundField 或 CheckBoxField 至 GridView 的每個方法所傳回的資料欄位。 其中一個資料欄位是`PriceQuartile`，這是我們所新增的資料行`ProductsDataTable`在步驟 1 中。

編輯移除的 GridView 的欄位以外的所有`ProductName`， `UnitPrice`，和`PriceQuartile`BoundFields。 設定`UnitPrice`格式化其值為貨幣，並讓 BoundField`UnitPrice`和`PriceQuartile`BoundFields 右邊-和置中對齊，分別。 最後，更新 剩餘 BoundFields`HeaderText`屬性，以產品、 價格和價格的四分位數，分別。 而且請檢查啟用排序 核取方塊的 GridView s 智慧標籤。

這些修改之後, 的 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

[圖 11] 顯示當透過瀏覽器瀏覽此頁面。 請注意，一開始，產品會按照其價格，以遞減順序，指派適當的各項產品的`PriceQuartile`值。 當然這項資料可依其他準則仍然反映價格方面的產品 s 順位的價格四分位數的資料行值 （請參閱 圖 12）。


[![其價格依排序產品](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**圖 11**: 產品會按照其價格 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image31.png))


[![依名稱排序的產品](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**圖 12**: 產品會依其名稱 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> 只要幾行程式碼我們可以增強 GridView，因此它不同色彩為基礎的產品資料列及其`PriceQuartile`值。 我們可能色彩中第一的四分位數淺綠色，第二個四分位數淺黃色中的這些產品等等。 建議您花點時間，以新增這項功能。 如果您需要複習一下格式化 GridView，請參閱[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-vb.md)教學課程。


## <a name="an-alternative-approach---creating-another-tableadapter"></a>替代方法-建立另一個的 TableAdapter

如我們所見在本教學課程中，將方法加入 TableAdapter 傳回以外的資料欄位拼出，主要的查詢時，我們可以加入至 DataTable 的對應資料行。 這種方式，不過，也只有少數的 TableAdapter 方法會傳回不同的資料欄位，以及這些替代的資料欄位不會改變太多來自主要查詢的運作方式。

而不是加入至 DataTable 的資料行，您可以改為包含從第一個的 TableAdapter 方法會傳回不同的資料欄位的資料集加入另一個 TableAdapter。 本教學課程中，而不是新增`PriceQuartile`資料行`ProductsDataTable`(其中它僅供`GetProductsWithPriceQuartile`方法)，我們可以加入額外的 TableAdapter 名為資料集`ProductsWithPriceQuartileTableAdapter`使用`Products_SelectWithPriceQuartile`儲存為其主要查詢的程序。 若要取得產品資訊價格的四分位數的所需的 ASP.NET 網頁會使用`ProductsWithPriceQuartileTableAdapter`，而不提供支援，可以繼續使用`ProductsTableAdapter`。

藉由新增新的 TableAdapter，Datatable 保持 untarnished 和其資料行精確地鏡像其 TableAdapter 的方法所傳回的資料欄位。 不過，重複的工作和功能，可以導入額外的 Tableadapter。 比方說，如果這些 ASP.NET 頁面顯示`PriceQuartile`資料行也需要提供 insert、 update 和 delete 支援`ProductsWithPriceQuartileTableAdapter`必須要有其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性正確設定。 雖然這些屬性會反映`ProductsTableAdapter`s，此組態會引進額外的步驟。 此外，現在有兩種方式可以更新，請刪除，或透過加入至資料庫的產品`ProductsTableAdapter`和`ProductsWithPriceQuartileTableAdapter`類別。

本教學課程中下載項目包括`ProductsWithPriceQuartileTableAdapter`類別中`NorthwindWithSprocs`說明此替代方法的資料集。

## <a name="summary"></a>總結

在大部分情況下，所有在 TableAdapter 方法會傳回相同的資料欄位，但有傳回一個額外欄位，有時可能的需要特定的方法或兩個。 例如，在[主要/詳細說明使用具有詳細資料 DataList 的主要記錄項目符號清單](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)教學課程中我們會新增方法以`CategoriesTableAdapter`，，除了主要查詢的資料欄位，傳回`NumberOfProducts`欄位報告每個類別目錄相關聯的產品數目。 在本教學課程中我們探討加入方法中的`ProductsTableAdapter`傳回`PriceQuartile`除了主要查詢的資料欄位的欄位。 若要擷取其他資料欄位所傳回的 TableAdapter 的方法我們要將對應的資料行加入至 DataTable。

如果您打算以手動方式將資料行加入至 DataTable，建議您使用 TableAdapter 使用預存程序。 如果 TableAdapter 會使用特定 SQL 陳述式，隨時 TableAdapter 組態精靈會執行的所有資料欄位清單還原為主查詢所傳回的資料欄位的方法。 這個問題不會延伸預存程序，這也是為什麼它們建議，並在本教學課程所使用。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Randy Schmidt、 Jacky Goor、 Bernadette Leigh 和 Hilton giesenow 將。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](updating-the-tableadapter-to-use-joins-vb.md)
> [下一頁](working-with-computed-columns-vb.md)
