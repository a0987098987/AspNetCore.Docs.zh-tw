---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: "加入其他 DataTable 資料行 (C#) |Microsoft 文件"
author: rick-anderson
description: "當使用 TableAdapter 精靈來建立具類型資料集，對應的資料表會包含主要資料庫查詢所傳回的資料行。 但有..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 2982af897b433706889cb4eda79dcb4e76baea62
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="adding-additional-datatable-columns-c"></a>加入其他 DataTable 資料行 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip)或[下載 PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> 當使用 TableAdapter 精靈來建立具類型資料集，對應的資料表會包含主要資料庫查詢所傳回的資料行。 但有時候，當 DataTable 需要包含其他資料行。 本教學課程中我們了解為什麼預存程序會建議使用我們需要其他 DataTable 資料行。


## <a name="introduction"></a>簡介

將 TableAdapter 加入至型別資料集，對應的 DataTable s 結構描述是 TableAdapter s 主查詢所決定。 例如，如果主要查詢傳回的資料欄位*A*， *B*，和*C*，資料表會有三個對應的資料行名為*A*，*B*，和*C*。除了其主要的查詢，TableAdapter 還可以包含其他查詢，傳回，有時也需要根據一些參數的資料子集。 比方說，除了以`ProductsTableAdapter`s 主查詢，傳回所有產品的相關資訊，它也包含如方法`GetProductsByCategoryID(categoryID)`和`GetProductByProductID(productID)`，這會傳回特定產品資訊，根據提供的參數。

如果所有 TableAdapter 的方法會傳回相同或較少的資料欄位，與主查詢中指定的運作方式有 s DataTable 的結構描述已反映 TableAdapter s 主查詢的模型。 如果 TableAdapter 方法必須傳回其他資料欄位，然後我們應該展開 DataTable s 結構描述據此。 在[主要/詳細說明使用主要記錄項目符號清單與詳細資料 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教學課程中，我們加入方法以`CategoriesTableAdapter`傳回`CategoryID`， `CategoryName`，和`Description`中定義的資料欄位主要的查詢加上`NumberOfProducts`，可報告的每個類別目錄相關聯的產品數目的其他資料欄位。 我們以手動方式加入至新的資料行`CategoriesDataTable`才能擷取`NumberOfProducts`資料欄位值從這個新方法。

中所述[上傳檔案](../working-with-binary-files/uploading-files-cs.md)教學課程，但必須小心使用 Tableadapter 使用特定 SQL 陳述式，且其資料欄位不精確地符合主要查詢的方法。 如果重新執行 TableAdapter 組態精靈，它會更新所有 TableAdapter 的方法，使其資料的欄位清單符合主查詢。 因此，任何方法使用自訂的資料行清單會還原成主查詢 s 資料行清單，而且不會傳回預期的資料。 使用預存程序時，就不會發生此問題。

在此教學課程中我們將探討如何擴充 DataTable s 結構描述，以包含其他資料行。 使用特定 SQL 陳述式時的 tableadapter 損毀，因為在此教學課程中我們將使用預存程序。 請參閱[建立新預存程序型別資料集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)和[使用現有預存程序型別資料集 s Tableadapter](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs)教學課程，如需詳細資訊設定成使用預存程序的 TableAdapter。

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>步驟 1： 加入`PriceQuartile`資料行`ProductsDataTable`

在*建立新預存程序型別資料集 s Tableadapter*教學課程中我們建立了名為具類型資料集`NorthwindWithSprocs`。 此資料集目前包含兩個 Datatable:`ProductsDataTable`和`EmployeesDataTable`。 `ProductsTableAdapter`具有下列三種方法：

- `GetProducts`-主要的查詢傳回的所有記錄`Products`資料表
- `GetProductsByCategoryID(categoryID)`-傳回具有指定的所有產品*categoryID*。
- `GetProductByProductID(productID)`-傳回指定的特定產品*productID*。

所有主要的查詢和兩個額外的方法傳回同一組資料欄位，也就是所有資料行從`Products`資料表。 有任何相互關聯子查詢或`JOIN`提取相關的資料之來源的 s`Categories`或`Suppliers`資料表。 因此，`ProductsDataTable`對應的資料行中每個欄位`Products`資料表。

本教學課程，可讓 s 將方法加入`ProductsTableAdapter`名為`GetProductsWithPriceQuartile`傳回所有產品。 除了標準產品資料欄位中，`GetProductsWithPriceQuartile`也會包含`PriceQuartile`資料欄位，表示產品的價格落在哪一個四分位數。 比方說，這些產品價格位於最高成本的 25%將會有`PriceQuartile`值為 1，雖然這些落在底部 25%的價格都有的值為 4。 我們會擔心建立預存程序，傳回此資訊之前，不過，我們先更新`ProductsDataTable`要包含的資料行來保存`PriceQuartile`結果時`GetProductsWithPriceQuartile`使用方法。

開啟`NorthwindWithSprocs`資料集，以滑鼠右鍵按一下`ProductsDataTable`。 從內容功能表選擇 加入，然後選擇 資料行。


[![將新的資料行加入至 ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**圖 1**： 加入新的資料行， `ProductsDataTable` ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image3.png))


這將會加入新的資料行名為型別的 Column1 DataTable `System.String`。 我們需要更新此資料行的名稱 PriceQuartile 和其類型為`System.Int32`因為它會用來保存 1 和 4 之間的數字。 選取新加入資料行`ProductsDataTable`，然後從 屬性 視窗中，設定`Name`PriceQuartile 的屬性和`DataType`屬性`System.Int32`。


[![設定新的資料行的名稱和資料類型屬性](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**圖 2**： 設定新的資料行 s`Name`和`DataType`屬性 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image6.png))


如圖 2 所示，有額外的屬性可以設定，例如資料行中的值是否必須是唯一的如果資料行自動遞增資料行，不論資料庫`NULL`值允許，依此類推。 請保留這些值設定為其預設值。

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>步驟 2： 建立`GetProductsWithPriceQuartile`方法

既然`ProductsDataTable`已更新為包含`PriceQuartile`資料行中，我們就可以準備建立`GetProductsWithPriceQuartile`方法。 啟動 TableAdapter 上按一下滑鼠右鍵，然後從內容功能表中選擇 加入查詢。 這會開啟 TableAdapter 查詢組態精靈中，先提示我們是否我們要使用特定 SQL 陳述式或新的或現有的預存程序。 因為我們不尚未傳回價格四分位數資料的預存程序，可讓 s 允許建立此預存程序為我們的 TableAdapter。 選取 建立新的預存程序選項，然後按一下 下一步。


[![指示 TableAdapter 精靈以建立預存程序，讓我們](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**圖 3**： 指示 TableAdapter 精靈以建立預存程序為我們 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image9.png))


在後續畫面中，顯示在圖 4 中，精靈會詢問我們要加入查詢的類型。 因為`GetProductsWithPriceQuartile`方法會傳回所有資料行和來自記錄`Products`資料表中，選取 選取會傳回資料列的選項，然後按一下 下一步。


[![我們的查詢將 SELECT 陳述式的傳回多個資料列](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**圖 4**： 我們的查詢將會`SELECT`陳述式的傳回多個資料列 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image12.png))


接著我們會提示輸入`SELECT`查詢。 在精靈中輸入下列查詢：


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

上述查詢會使用 SQL Server 2005 s 新[`NTILE`函式](https://msdn.microsoft.com/library/ms175126.aspx)將結果分成四個群組的群組由決定`UnitPrice`值以遞減順序排序。

不幸的是，查詢產生器不知道如何剖析`OVER`關鍵字剖析上述查詢時，就會顯示錯誤。 因此，請輸入上述查詢直接在文字方塊中，在精靈中，而不需使用查詢產生器。

> [!NOTE]
> 如需有關 NTILE 和 SQL Server 2005 s 其他排名函數，請參閱[傳回等級結果與 Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml)和[排名函數區段](https://msdn.microsoft.com/library/ms189798.aspx)從[SQLServer 2005 線上叢書 》](https://msdn.microsoft.com/library/ms189798.aspx)。


輸入後`SELECT`查詢並按一下 [下一步]，精靈會詢問我們提供，它會建立預存程序的名稱。 命名新的預存程序`Products_SelectWithPriceQuartile`，按一下 [下一步]。


[![預存程序 Products_SelectWithPriceQuartile 名稱](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**圖 5**： 命名預存程序`Products_SelectWithPriceQuartile`([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image15.png))


最後，我們會提示您命名的 TableAdapter 方法。 保留的填滿 DataTable 和傳回 DataTable 核取方塊，檢查和名稱的方法，`FillWithPriceQuartile`和`GetProductsWithPriceQuartile`。


[![名稱的 tableadapter 方法，然後按一下 [完成]](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**圖 6**： 命名 TableAdapter 的方法並按一下 [完成] ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image18.png))


與`SELECT`指定查詢和預存程序和 TableAdapter 方法，名為，按一下 [完成]，以完成精靈。 此時您可能會收到警告，或從精靈所說的兩個`OVER`不支援 SQL 建構或陳述式。 您可以忽略這些警告。

完成精靈之後，應該包含 TableAdapter`FillWithPriceQuartile`和`GetProductsWithPriceQuartile`應該包括名為預存程序的方法和資料庫`Products_SelectWithPriceQuartile`。 請花一點時間來驗證，TableAdapter 確實包含這個新的方法和，預存程序已正確加入至資料庫。 如果您看不到預存程序再試一次滑鼠右鍵按一下 預存程序 資料夾並選擇 重新整理，請檢查資料庫。


![請確認 TableAdapter 中已加入新的方法](adding-additional-datatable-columns-cs/_static/image19.png)

**圖 7**： 請確認 TableAdapter 中已加入新的方法


[![請確認資料庫包含 Products_SelectWithPriceQuartile 預存程序](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**圖 8**： 請確認資料庫包含`Products_SelectWithPriceQuartile`預存程序 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> 使用預存程序，而不特定 SQL 陳述式的優點是，重新執行 TableAdapter 組態精靈不會修改預存程序資料行清單。 驗證，請在 TableAdapter 上按一下滑鼠右鍵，從內容功能表啟動精靈，選擇 [設定] 選項，然後按一下 [完成] 來完成它。 接下來，請移至資料庫，然後檢視`Products_SelectWithPriceQuartile`預存程序。 請注意不修改其資料行清單。 我們一直都使用特定 SQL 陳述式，重新執行 TableAdapter 組態精靈會還原此查詢 s 資料行清單，以符合主要查詢資料行清單中，藉此移除 NTILE 陳述式所使用的查詢從`GetProductsWithPriceQuartile`方法。


當資料存取層 s`GetProductsWithPriceQuartile`叫用方法、 TableAdapter 執行`Products_SelectWithPriceQuartile`預存程序，並加入資料列來`ProductsDataTable`的每個傳回的記錄。 預存程序所傳回的資料欄位會對應至`ProductsDataTable`s 資料行。 因為沒有`PriceQuartile`資料欄位所傳回的預存程序，其值會指派給`ProductsDataTable`s`PriceQuartile`資料行。

其查詢不會傳回這些 TableAdapter 方法`PriceQuartile`資料欄位`PriceQuartile`資料行的值是所指定的值及其`DefaultValue`屬性。 如圖 2 所示，此值設定為`DBNull`，預設值。 如果您想使用不同的預設值，只要設定`DefaultValue`屬性據此。 只確定`DefaultValue`值是有效的指定資料行 s `DataType` (亦即，`System.Int32`如`PriceQuartile`資料行)。

此時我們可以將額外的資料行加入至 DataTable 執行必要的步驟。 若要確認此額外的資料行可以正常運作，讓建立 ASP.NET 網頁，會顯示每個產品的名稱、 價格和價格的四分位數。 這樣做之前，不過，我們先需要更新商務邏輯層，以納入方法呼叫向下到 DAL 的`GetProductsWithPriceQuartile`方法。 我們將更新 BLL 接下來，在步驟 3 中，然後在步驟 4 中建立 ASP.NET 網頁。

## <a name="step-3-augmenting-the-business-logic-layer"></a>步驟 3： 加強商務邏輯層

我們使用新之前`GetProductsWithPriceQuartile`方法從展示層中，我們應該先將對應的方法加入 BLL。 開啟`ProductsBLLWithSprocs`類別檔案，然後將下列程式碼：


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

中的其他資料擷取方法一樣`ProductsBLLWithSprocs`、`GetProductsWithPriceQuartile`方法只會呼叫 DAL s 對應`GetProductsWithPriceQuartile`方法，並傳回其結果。

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>步驟 4: ASP.NET Web 網頁中顯示的價格四分位數資訊

加上 BLL 完成我們準備好要建立 ASP.NET 頁面，其中顯示每項產品的價格四分位數。 開啟`AddingColumns.aspx`頁面`AdvancedDAL`資料夾，然後拖曳 GridView 從工具箱拖曳至設計工具中，設定其`ID`屬性`Products`。 從 GridView s 智慧標籤，將它繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定用於 ObjectDataSource`ProductsBLLWithSprocs`類別的`GetProductsWithPriceQuartile`方法。 因為這會是唯讀的方格，設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**圖 9**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image25.png))


[![擷取產品資訊從 GetProductsWithPriceQuartile 方法](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**圖 10**： 擷取產品資訊`GetProductsWithPriceQuartile`方法 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image28.png))


完成設定資料來源精靈之後，Visual Studio 會自動加入 BoundField 或 CheckBoxField 至 GridView 每個方法所傳回的資料欄位。 一個資料欄位是`PriceQuartile`，這是我們要加入的資料行`ProductsDataTable`在步驟 1 中。

編輯 GridView 的欄位中，移除以外的所有`ProductName`， `UnitPrice`，和`PriceQuartile`BoundFields。 設定`UnitPrice`格式化為貨幣值，並讓 BoundField`UnitPrice`和`PriceQuartile`BoundFields 右邊的並置中對齊，分別。 最後，更新 剩餘 BoundFields`HeaderText`屬性，以產品、 價格和價格的四分位數，分別。 另請檢查 GridView s 智慧標籤啟用排序核取方塊。

這些修改之後, GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

圖 11 顯示當透過瀏覽器瀏覽此頁面。 請注意，一開始，已訂購的產品依照依遞減順序，與指派適當的每項產品其價格`PriceQuartile`值。 當然此資料可以依其他準則價格四分位數的資料行值仍反映相對於價格的產品 s 順位 （請參閱圖 12）。


[![產品被依其價格](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**圖 11**: 產品會依其價格 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image31.png))


[![名稱來排序產品](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**圖 12**: 產品會依其名稱 ([按一下以檢視完整大小的影像](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> 只要幾行程式碼我們無法擴大 GridView，因此它不同色彩為基礎的產品資料列及其`PriceQuartile`值。 我們可能色彩中第一個四分位數淺綠色，在第二個四分位數淺黃色，這些產品等等。 建議您花點時間加入這項功能。 如果您需要格式化的 GridView 的重新整理程式，請參閱[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-cs.md)教學課程。


## <a name="an-alternative-approach---creating-another-tableadapter"></a>替代的方法-建立其他的 TableAdapter

如我們所見在本教學課程中，將方法加入 TableAdapter 傳回以外的資料欄位的主查詢拼時，我們可以將對應的資料行加入資料表。 這種方法，不過，運作良好而且太多來自主要查詢不會改變這些替代資料欄位才有少數的 TableAdapter 方法會傳回不同的資料欄位。

而不是將資料行加入至 DataTable，您可以改為包含從第一個 TableAdapter 方法會傳回不同的資料欄位的資料集加入另一個 TableAdapter。 此教學課程中，而不用新增`PriceQuartile`資料行`ProductsDataTable`(只使用該`GetProductsWithPriceQuartile`方法)，我們無法加入其他的 TableAdapter 名為資料集`ProductsWithPriceQuartileTableAdapter`用`Products_SelectWithPriceQuartile`儲存作為其主要查詢的程序。 若要取得產品資訊的價格四分位數所需的 ASP.NET 網頁會使用`ProductsWithPriceQuartileTableAdapter`，而不提供支援，無法繼續使用`ProductsTableAdapter`。

藉由加入新的 TableAdapter，Datatable 保持 untarnished，且其資料行明確地說鏡像其 TableAdapter 的方法所傳回的資料欄位。 不過，其他的 Tableadapter 會造成重複的工作和功能。 比方說，如果這些 ASP.NET 頁面顯示`PriceQuartile`資料行也需要提供 insert、 update 和 delete 支援`ProductsWithPriceQuartileTableAdapter`需要其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性正確設定。 雖然這些屬性會鏡像`ProductsTableAdapter`s，此組態導入額外的步驟。 此外，還有現在更新，兩種方式刪除，或透過將產品加入到資料庫-`ProductsTableAdapter`和`ProductsWithPriceQuartileTableAdapter`類別。

在此教學課程下載包含`ProductsWithPriceQuartileTableAdapter`類別`NorthwindWithSprocs`說明這個替代方法的資料集。

## <a name="summary"></a>總結

在大部分情況下，所有在 TableAdapter 方法會傳回相同的整組資料欄位，但有要傳回的其他欄位有時可能的需要特定的方法或兩個。 例如，在[主要/詳細說明使用主要記錄項目符號清單與詳細資料 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教學課程中，我們加入方法以`CategoriesTableAdapter`，除了主要查詢的資料欄位，傳回`NumberOfProducts`欄位報告的每個類別目錄相關聯的產品數目。 在此教學課程中我們探討加入方法中的`ProductsTableAdapter`傳回`PriceQuartile`除了主要查詢的資料欄位的欄位。 要擷取的其他資料欄位的 TableAdapter 的方法傳回我們要將對應的資料行加入至 DataTable。

如果您計劃手動將資料行加入至 DataTable，建議您使用 TableAdapter 使用預存程序。 如果 TableAdapter 會使用特定 SQL 陳述式，隨時 TableAdapter 組態精靈會執行所有的還原到主查詢所傳回的資料欄位的資料欄位清單的方法。 這個問題不會擴充預存程序，這就是為什麼這些建議與本教學課程中所使用。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已袁 Schmidt、 Jacky Goor、 Bernadette Leigh 和 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](updating-the-tableadapter-to-use-joins-cs.md)
[下一頁](working-with-computed-columns-cs.md)
