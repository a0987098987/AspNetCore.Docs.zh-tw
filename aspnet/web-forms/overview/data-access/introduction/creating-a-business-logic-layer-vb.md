---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: 建立商務邏輯層 (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們會看到如何集中管理您的商務規則到商務邏輯層 (BLL) 做為 t 之間交換資料的媒介...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 150862decbbb69747f3e957a941b71b118b7231c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892760"
---
<a name="creating-a-business-logic-layer-vb"></a>建立商務邏輯層 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe)或[下載 PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> 在本教學課程，我們會看到如何集中化商務規則到商務邏輯層 (BLL) 做為在展示層和 DAL 之間交換資料的媒介。


## <a name="introduction"></a>簡介

資料存取層 (DAL) 中建立[第一個教學課程](creating-a-data-access-layer-vb.md)完全分隔資料存取邏輯從呈現邏輯。 不過，DAL 清楚分隔資料的存取詳細資料，從展示層，而它不會強制執行可能需支付任何商務規則。 比方說，應用程式可能要禁止`CategoryID`或`SupplierID`欄位`Products`資料表修改時`Discontinued`欄位設定為 1，或我們可能會想要強制執行 seniority 規則禁止情況員工受管理之後其雇用的人員。 另一個常見案例是授權可能只有特定角色中的使用者可以刪除產品，或可以變更`UnitPrice`值。

在本教學課程中，我們會看到如何集中化這些商務規則到商務邏輯層 (BLL) 做為在展示層和 DAL 之間交換資料的媒介。 在真實世界應用程式中，BLL 應該實作為個別的類別庫專案。不過，這些教學課程中我們會實作 BLL 為一系列中的類別我們`App_Code`資料夾，才能簡化的專案結構。 圖 1 所示之間展示層、 BLL，DAL 架構的關聯性。


![BLL 資料存取層的分隔展示層，並會加諸的商務規則](creating-a-business-logic-layer-vb/_static/image1.png)

**圖 1**: BLL 資料存取層的分隔展示層，並會加諸的商務規則


而不是建立個別的類別以實作我們[商務邏輯](http://en.wikipedia.org/wiki/Business_logic)，我們無法或者置於此邏輯直接輸入資料集部分類別。 如需建立和擴充輸入資料集的範例，請參閱回第一個教學課程。

## <a name="step-1-creating-the-bll-classes"></a>步驟 1： 建立 BLL 類別

我們 BLL 將由四個類別，一個用於每個 TableAdapter 中 DAL;每個 BLL 類別會有用於擷取、 插入、 更新和刪除 DAL 套用適當的商務規則的個別 TableAdapter 方法。

若要更清楚分隔 DAL 和 BLL 相關的類別，讓我們來建立兩個子資料夾中的`App_Code`資料夾，`DAL`和`BLL`。 只要以滑鼠右鍵按一下`App_Code`資料夾在 [方案總管] 中的選擇新的資料夾。 建立之後這兩個資料夾，請移至第一個教學課程中建立的具類型資料集`DAL`子資料夾。

接下來，建立四個 BLL 類別檔案中的`BLL`子資料夾。 若要完成此動作，以滑鼠右鍵按一下`BLL`子資料夾，選擇 加入新項目，然後選擇 在類別樣板。 名稱的四個類別`ProductsBLL`， `CategoriesBLL`， `SuppliersBLL`，和`EmployeesBLL`。


![將四個新類別加入至 App_Code 資料夾](creating-a-business-logic-layer-vb/_static/image2.png)

**圖 2**： 新增四個新的類別來`App_Code`資料夾


接下來，我們將方法加入至每個類別包裝 Tableadapter 從第一個教學課程所定義的方法。 現在，這些方法會直接呼叫直接插入 DAL;我們將稍後加入任何所需的商務邏輯。

> [!NOTE]
> 如果您使用 Visual Studio Standard 版或更新版本 (也就是您*不*使用 Visual Web Developer)，您可以選擇性地設計您的類別，以視覺化方式使用[類別設計工具](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)。 請參閱[類別設計工具部落格](https://blogs.msdn.com/classdesigner/default.aspx)如需有關這個 Visual Studio 中的新功能。


如`ProductsBLL`我們需要加入總共七種方法的類別：

- `GetProducts()` 傳回所有產品
- `GetProductByProductID(productID)` 傳回指定的產品識別碼的產品
- `GetProductsByCategoryID(categoryID)` 傳回所有產品，從指定的類別目錄
- `GetProductsBySupplier(supplierID)` 傳回所有產品，從指定的供應商
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` 使用值的資料庫中插入新產品傳入;傳回`ProductID`新插入的記錄值
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` 更新使用傳入的值; 在資料庫中現有的產品傳回`True`精確一個資料列已更新，如果`False`否則
- `DeleteProduct(productID)` 從資料庫刪除指定的產品

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

只傳回資料的方法`GetProducts`， `GetProductByProductID`， `GetProductsByCategoryID`，和`GetProductBySuppliersID`都相當直接明瞭，因為它們只是呼叫向下 DAL。 在某些情況下可能需要實作的商務規則，而此層級 （例如目前登入的使用者或使用者所隸屬的角色為基礎的授權規則），我們只會保留這些方法為-是。 這些方法，然後 BLL 做只是透過此展示分層會存取基礎資料從資料存取層的 proxy。

`AddProduct`和`UpdateProduct`方法同時採用做為參數的值不同的產品欄位和加入新的產品或更新現有磁碟，分別。 因為許多`Product`資料表的資料行可以接受`NULL`值 (`CategoryID`， `SupplierID`，和`UnitPrice`，以提供幾個)，這些輸入參數`AddProduct`和`UpdateProduct`對應至這類資料行使用[可為 null 的型別](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)。 可為 null 的型別不熟悉.NET 2.0 和提供的技術，指出是否實值型別應該反而是`Nothing`。 是指[Paul Vick](http://www.panopticoncentral.net/)的部落格項目[真關於可為 Null 類型和 VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx)和技術文件[Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx)結構，如需詳細資訊。

所有的三個方法都會傳回布林值，指出資料列已插入、 更新或刪除，因為作業可能不會導致受影響的資料列。 例如，如果網頁開發人員呼叫`DeleteProduct`傳入`ProductID`不存在的產品，`DELETE`發出至資料庫的陳述式不會有作用，因此`DeleteProduct`方法會傳回`False`。

請注意，當加入新的產品，或更新現有我們採用中新的或修改產品的欄位值做為純量相對於接受一份`ProductsRow`執行個體。 已選擇這種方法，因為`ProductsRow`類別衍生自 ADO.NET`DataRow`類別，沒有預設無參數建構函式。 若要建立新`ProductsRow`執行個體，我們必須先建立`ProductsDataTable`執行個體，然後叫用其`NewProductRow()`方法 (我們對執行`AddProduct`)。 當我們要插入和更新產品使用 ObjectDataSource 開啟時，這個缺點 rears 其標頭。 簡單地說，ObjectDataSource 會嘗試建立的輸入參數的執行個體。 如果 BLL 方法預期`ProductsRow`，ObjectDataSource 會嘗試執行個體建立一個，但由於缺少預設無參數建構函式會失敗。 如需有關這個問題的詳細資訊，請參閱下列兩個 ASP.NET 論壇文章： [Strongly-Typed 資料集的更新 ObjectDataSources](https://forums.asp.net/1098630/ShowPost.aspx)，和[問題與 ObjectDataSource 和 Strongly-Typed 資料集](https://forums.asp.net/1048212/ShowPost.aspx).

接下來，在同時`AddProduct`和`UpdateProduct`，程式碼會建立`ProductsRow`執行個體，並填入只傳入的值。 將值指派給資料列的 DataColumns 時可能會發生各種欄位層級的驗證檢查。 因此，以手動方式將傳入的值傳回放入 DataRow，有助於確保 BLL 方法傳遞資料的有效性。 不幸的是 Visual Studio 所產生的強型別 DataRow 類別不會使用可為 null 的類型。 而是表示資料列中的特定 DataColumn 應該對應至`NULL`資料庫的值，我們必須使用`SetColumnNameNull()`方法。

在`UpdateProduct`我們先載入要使用更新的產品`GetProductByProductID(productID)`。 這似乎不必要的往返資料庫，而此額外的往返將證明值得在未來教學課程中，瀏覽開放式並行存取。 開放式並行存取是一種技術確保兩個使用者同時使用相同的資料不小心不要覆寫彼此的變更。 抓取整個記錄也可讓您更輕鬆地建立更新方法中只能修改 DataRow 的資料行的子集 BLL。 當我們探討`SuppliersBLL`我們會看到這類範例的類別。

最後，請注意，`ProductsBLL`類別具有[DataObject 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx)套用 (`[System.ComponentModel.DataObject]`前類別陳述式，在檔案頂端附近的語法) 的方法具有與[DataObjectMethodAttribute 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)。 `DataObject`屬性標記為適合的繫結的物件類別[ObjectDataSource 控制項](https://msdn.microsoft.com/library/9a4kyhcx.aspx)，而`DataObjectMethodAttribute`表示方法的目的。 如稍後將在未來教學課程，ASP.NET 2.0 ObjectDataSource 可讓您輕鬆地以宣告方式從類別中存取資料。 若要篩選的繫結至 ObjectDataSource 精靈中的可能類別清單，依預設只將標示為這些類別`DataObjects`精靈的下拉式清單中會顯示。 `ProductsBLL`類別將同樣也適用於不含這些屬性，但將它們加入可以更輕鬆地在 ObjectDataSource 精靈中使用。

## <a name="adding-the-other-classes"></a>加入其他類別

與`ProductsBLL`完整的類別，我們仍需要加入類別目錄、 供應商和員工所使用的類別。 請花一點時間來建立下列類別和方法，使用上述範例中的概念：

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

值得注意的其中一個方法是`SuppliersBLL`類別的`UpdateSupplierAddress`方法。 這個方法會提供介面，用於更新就只是供應商的位址資訊。 就內部而言，這個方法會讀取中`SupplierDataRow`物件指定`supplierID`(使用`GetSupplierBySupplierID`)、 設定其地址相關屬性，並接著呼叫向下到`SupplierDataTable`的`Update`方法。 `UpdateSupplierAddress`方法如下所示：


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

請參閱這篇文章下載 BLL 類別的完整實作。

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>步驟 2： 透過 BLL 類別存取具類型資料集

第一個教學課程中我們可了解範例的直接輸入資料集以程式設計方式使用，但我們 BLL 類別加入，展示層應該工作針對 BLL 改為。 在`AllProducts.aspx`從第一個教學課程中，範例`ProductsTableAdapter`用來將產品的清單繫結至 GridView，如下列程式碼所示：


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

若要使用新 BLL 類別，所有需要的變更是第一行程式碼只取代`ProductsTableAdapter`物件`ProductBLL`物件：


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

BLL 類別也可以使用 ObjectDataSource 存取以宣告方式 （如可輸入資料集）。 我們將在下列的教學課程中討論更詳細地 ObjectDataSource。


[![產品的清單會顯示在 GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**圖 3**： 產品的清單會顯示在 GridView ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>步驟 3： 加入 DataRow 類別欄位層級驗證

欄位層級驗證會檢查與插入或更新時的商務物件的屬性值。 產品有些欄位層級驗證規則包括：

- `ProductName`欄位必須是 40 個字元長度等於或小於
- `QuantityPerUnit`欄位必須是 20 個字元長度等於或小於
- `ProductID`， `ProductName`，和`Discontinued`欄位是必要的但其他所有欄位都是選用
- `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`欄位必須是大於或等於零

這些規則可以而且應該以表示資料庫層級。 字元的限制`ProductName`和`QuantityPerUnit`欄位中資料行的資料型別，即可擷取`Products`資料表 (`nvarchar(40)`和`nvarchar(20)`分別)。 如果資料庫資料表資料行允許欄位是否為必要和選擇性來表示`NULL`s。 四個[check 條件約束](https://msdn.microsoft.com/library/ms188258.aspx)存在，請務必大於或等於零的值可以讓它`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`資料行。

除了強制執行這些規則，在資料庫中他們應該也會強制執行資料集層級。 事實上，欄位長度值是必要或選擇性已擷取和每個 DataTable DataColumns 組。 自動提供的現有欄位層級驗證，請移至 DataSet 設計工具從 Datatable 的其中一個選取的欄位，然後移至 屬性 視窗。 如圖 4 所示， `QuantityPerUnit` DataColumn 在`ProductsDataTable`20 個字元的最大長度，允許`NULL`值。 如果我們嘗試設定`ProductsDataRow`的`QuantityPerUnit`超過 20 個字元的字串值的屬性`ArgumentException`就會擲回。


[![DataColumn 提供基本的欄位層級驗證](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**圖 4**: DataColumn 提供基本欄位層級驗證 ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-vb/_static/image8.png))


不幸的是，我們無法指定界限檢查，例如`UnitPrice`值必須大於或等於零，透過 [屬性] 視窗。 若要提供這種類型的欄位層級驗證我們要建立的 DataTable 的事件處理常式[ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx)事件。 中所述[前述教學課程](creating-a-data-access-layer-vb.md)，可以透過使用部分類別擴充輸入資料集所建立的 DataSet、 Datatable 和 DataRow 物件。 使用這項技術，我們可以建立`ColumnChanging`事件處理常式`ProductsDataTable`類別。 藉由建立類別，以在啟動`App_Code`資料夾名為`ProductsDataTable.ColumnChanging.vb`。


[![將新類別加入至 App_Code 資料夾](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**圖 5**： 將新類別加入`App_Code`資料夾 ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-vb/_static/image11.png))


接下來，建立事件處理常式`ColumnChanging`事件，可確保`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`資料行值 (如果不是`NULL`) 大於或等於零。 如果這類資料行超出範圍時，會擲回`ArgumentException`。

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>步驟 4： 將自訂的商務規則加入至 BLL 類別

除了欄位層級驗證，可能牽涉到不同的實體或不可以表示的概念，在單一資料行層級，例如的高層級的自訂商務規則：

- 如果產品，已然中止其`UnitPrice`無法更新
- 員工所居住的國家必須他的經理所居住的國家相同
- 如果它是唯一供應商所提供的產品，無法停用產品

BLL 類別應該包含檢查，以確保應用程式的商務規則，請遵循。 這些檢查直接加入它們所套用的方法。

假設我們的商務規則聽寫的產品無法標示已停止是否只從指定的供應商產品。 亦即，如果產品*X*是唯一的產品，我們從供應商購買*Y*，我們無法將標記*X*為停用; 如果，不過，供應商*Y*提供三種產品， *A*， *B*，和*C*，則我們可以在任何標示，而且所有的這些做為已停止。 永遠不對齊有奇數的商務規則，但是商務規則和常識 ！

若要強制執行此商務規則中的`UpdateProducts`方法，我們會先檢查是否`Discontinued`已設為`True`，因此，我們會呼叫`GetProductsBySupplierID`我們購買此產品的供應商來判斷多少的產品。 如果只有一個產品為採購此供應商，我們會擲回`ApplicationException`。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>展示層中的驗證錯誤回應

當從展示層中呼叫 BLL 我們可以決定是否要嘗試處理可能會引發，或將這些反昇至 ASP.NET 的任何例外狀況 (這將會引發`HttpApplication`的`Error`事件)。 若要以程式設計方式使用 BLL 時處理例外狀況，我們可以使用[再試一次...攔截](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx)區塊，如下列範例所示：


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

我們會看到在未來教學課程、 處理例外狀況時所使用的資料時 BLL Web 控制項，插入、 更新或刪除資料可以直接在事件處理常式，而不需要換行中的程式碼中處理`Try...Catch`區塊。

## <a name="summary"></a>總結

設計完善的應用程式被製作成不同的層，其中每個封裝特定的角色。 本文章系列的第一個教學課程中我們建立了資料存取層使用具類型資料集;在此教學課程中我們建立商務邏輯層為一系列的類別在我們的應用程式中`App_Code`呼叫我們 DAL 的資料夾。 BLL 實作我們的應用程式的欄位層級和層級的商務邏輯。 除了建立個別的 BLL，如同我們在此教學課程中，另一個選項是擴充 Tableadapter 的方法，透過使用部分類別。 不過，使用這項技術不允許我們覆寫現有的方法，也不該分隔我們 DAL 和我們 BLL 為我們已經在本文中的方法可以正常。

DAL 和 BLL 完成，我們已準備好開始在我們展示層上。 在[下一個教學課程](master-pages-and-site-navigation-vb.md)我們將會從資料存取的主題簡要繞道，並定義在此教學課程中使用一致的頁面配置。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Liz Shulok、 Dennis Patterson、 Carlos Santos 和 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-a-data-access-layer-vb.md)
> [下一頁](master-pages-and-site-navigation-vb.md)
