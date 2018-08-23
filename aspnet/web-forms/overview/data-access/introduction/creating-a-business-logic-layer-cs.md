---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: 建立商業邏輯層 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到如何集中管理您的商務規則到商務邏輯層 (BLL)，做為媒介 t 之間交換資料...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db90f1e87bcaac51ca08ef1a8b258c93be8f613
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832216"
---
<a name="creating-a-business-logic-layer-c"></a>建立商業邏輯層 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe)或[下載 PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> 在本教學課程中，我們會看到如何集中管理您的商務規則到商務邏輯層 (BLL) 做為交換資料的展示層和 DAL 之間的媒介。


## <a name="introduction"></a>簡介

資料存取層 (DAL) 中建立[第一個教學課程](creating-a-data-access-layer-cs.md)完全分隔的資料存取邏輯和展示邏輯。 不過，雖然 DAL 清楚分隔的資料存取詳細資料，從展示層，它不會強制執行可能適用於任何商務規則。 比方說，我們的應用程式我們可能會想要禁止`CategoryID`或是`SupplierID`的欄位`Products`資料表，以修改`Discontinued`欄位設定為 1，或者我們可能想要強制執行 seniority 規則禁止情況下會員工是由他們雇用的人員管理。 另一個常見案例是授權可能只有特定角色中的使用者可以刪除產品，或是可以變更`UnitPrice`值。

在本教學課程中，我們會看到如何集中化這些商務規則到商務邏輯層 (BLL)，做為媒介的展示層和 DAL 之間交換資料。 在真實世界應用程式中，BLL 應該實作為個別的類別庫專案;不過，這些教學課程中我們將實作 BLL 一系列中的類別為我們`App_Code`資料夾，以簡化專案結構。 [圖 1] 說明之間的展示層，接著，BLL 和 DAL 架構的關聯性。


![BLL 會展示層分隔從資料存取層，並強制施行商務規則](creating-a-business-logic-layer-cs/_static/image1.png)

**圖 1**: BLL 會展示層分隔從資料存取層，並強制施行商務規則


## <a name="step-1-creating-the-bll-classes"></a>步驟 1： 建立 BLL 類別

我們的 BLL 將由四個類別，一個用於每個 DAL; 中的 TableAdapter每個 BLL 類別必須用於擷取、 插入、 更新和刪除從 DAL 中，將適當的商務規則套用在個別的 TableAdapter 方法。

更完全分隔的 DAL 和 BLL 相關的類別，讓我們建立兩個資料夾中的子`App_Code`資料夾中，`DAL`和`BLL`。 只要以滑鼠右鍵按一下`App_Code`方案總管] 中的資料夾，然後選擇 [新資料夾。 建立這兩個資料夾之後, 移動到第一個教學課程中建立的具類型資料集`DAL`子資料夾。

接下來，建立四個 BLL 類別檔案，在`BLL`子資料夾。 若要這麼做，以滑鼠右鍵按一下`BLL`子資料夾中，新的項目中，選擇 [新增]，然後選擇 [類別] 範本。 名稱的四個類別`ProductsBLL`， `CategoriesBLL`， `SuppliersBLL`，和`EmployeesBLL`。


![將四個新類別加入至 App_Code 資料夾](creating-a-business-logic-layer-cs/_static/image2.png)

**圖 2**： 新增四個新的類別，來`App_Code`資料夾


接下來，我們將方法加入至每個類別只是換行定義為第一個教學課程從 Tableadapter 的方法。 現在，這些方法會只是直接呼叫 DAL;我們會傳回更新版本，才能將任何所需的商務邏輯加入項目。

> [!NOTE]
> 如果您使用 Visual Studio Standard Edition 或更新版本 (也就是你*未*使用 Visual Web Developer)，您可以選擇性地設計您以視覺化方式使用的類別[類別設計工具](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)。 請參閱[類別設計工具部落格](https://blogs.msdn.com/classdesigner/default.aspx)如需有關在 Visual Studio 中這項新功能。


針對`ProductsBLL`我們需要加入總共七種方法的類別：

- `GetProducts()` 會傳回所有產品
- `GetProductByProductID(productID)` 傳回指定的產品識別碼的產品
- `GetProductsByCategoryID(categoryID)` 從指定的類別會傳回所有產品
- `GetProductsBySupplier(supplierID)` 傳回從指定的供應商的所有產品
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` 將新產品插入至資料庫使用的值傳入;傳回`ProductID`新插入的資料錄的值
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` 更新現有的產品使用傳入的值; 資料庫中會傳回`true`更新一個資料列，如果`false`否則
- `DeleteProduct(productID)` 從資料庫刪除指定的產品

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

只傳回資料的方法`GetProducts`， `GetProductByProductID`， `GetProductsByCategoryID`，和`GetProductBySuppliersID`都相當直接明瞭，因為它們只是呼叫向下 DAL。 雖然在某些情況下可能需要實作的商務規則在此層級 （例如目前登入的使用者或使用者所屬的角色為基礎的授權規則），我們只是要將這些方法為是。 如需這些方法，然後，BLL 做只是讓展示層會存取基礎資料從資料存取層的 proxy。

`AddProduct`和`UpdateProduct`方法同時採用做為參數的值為各個不同的產品欄位和加入新的產品或更新現有的帳戶，分別。 因為許多`Product`資料表的資料行可以接受`NULL`值 (`CategoryID`， `SupplierID`，和`UnitPrice`，等等)，這些輸入參數`AddProduct`和`UpdateProduct`對應到此類資料行使用[可為 null 的型別](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)。 可為 null 的型別不熟悉.NET 2.0 和提供一種技術，指出是否實值型別，反而是`null`。 在 C# 中您可以設定旗標，實值型別為 null 的型別加入`?`類型 (例如`int? x;`)。 請參閱[可為 Null 的型別](https://msdn.microsoft.com/library/1t3y8s4s.aspx)一節[C# 程式設計手冊](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx)如需詳細資訊。

所有的三個方法都會傳回布林值，指出資料列已插入、 更新或刪除，因為作業可能不會導致受影響的資料列。 比方說，如果頁面開發人員會呼叫`DeleteProduct`傳入`ProductID`不存在產品`DELETE`發出給資料庫的陳述式會產生任何影響，因此`DeleteProduct`方法會傳回`false`。

請注意，當加入新的產品，或更新現有我們採用新的或修改產品的欄位值中做為純量，而不是接受一份`ProductsRow`執行個體。 已選擇這種方法，因為`ProductsRow`類別衍生自 ADO.NET`DataRow`類別，沒有預設無參數建構函式。 若要建立新`ProductsRow`執行個體，我們必須先建立`ProductsDataTable`執行個體，然後叫用其`NewProductRow()`方法 (我們完成此動作`AddProduct`)。 當我們開啟來插入及更新使用 ObjectDataSource 的產品時，這個缺點 rears 其標頭。 簡單地說，ObjectDataSource 會嘗試建立的輸入參數的執行個體。 如果 BLL 方法預期`ProductsRow`，ObjectDataSource 會嘗試執行個體建立一個，但因為缺乏預設無參數建構函式會失敗。 如需有關此問題的詳細資訊，請參閱下列的兩個 ASP.NET 論壇文章： [Strongly-Typed 資料集的更新 ObjectDataSources](https://forums.asp.net/1098630/ShowPost.aspx)，和[問題與 ObjectDataSource 和 Strongly-Typed 資料集](https://forums.asp.net/1048212/ShowPost.aspx).

接下來，在兩者`AddProduct`並`UpdateProduct`，程式碼會建立`ProductsRow`執行個體，並填入只傳入的值。 將值指派給 DataRow 的 DataColumns 時可能會發生各種欄位層級的驗證檢查。 因此，以手動方式將傳入的值重新放入 DataRow，有助於確保 BLL 方法所傳遞之資料的有效性。 不幸的是 Visual Studio 所產生的強型別 DataRow 類別不會使用可為 null 的型別。 相反地，以指出特定的 DataColumn DataRow 中應該對應至`NULL`資料庫的值，我們必須使用`SetColumnNameNull()`方法。

在 `UpdateProduct`我們第一次載入要使用更新的產品`GetProductByProductID(productID)`。 雖然這看起來好像不需要來回存取資料庫，則會有此額外的行程證明值得在未來教學課程中，瀏覽開放式並行存取。 開放式並行存取是一種技術，以確保，同時使用相同的資料的兩位使用者不小心覆寫他人的變更。 抓取整筆記錄也可讓您更輕鬆地只修改 DataRow 的資料行的子集到 BLL 中建立更新方法。 當我們將探討`SuppliersBLL`我們會看到這類範例的類別。

最後，請注意，`ProductsBLL`類別具有[DataObject 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx)套用 (`[System.ComponentModel.DataObject]`之前 class 陳述式，該檔案頂端附近的語法) 及方法有[DataObjectMethodAttribute 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)。 `DataObject`屬性標記的類別視為適合繫結至物件[ObjectDataSource 控制項](https://msdn.microsoft.com/library/9a4kyhcx.aspx)，而`DataObjectMethodAttribute`表示方法的目的。 誠如所見未來教學課程，ASP.NET 2.0 的 ObjectDataSource 可讓您輕鬆地以宣告方式從類別中存取資料。 若要篩選的 ObjectDataSource 的精靈中的繫結至可能的類別清單，依預設只將標示為這些類別`DataObjects`精靈的下拉式清單中會顯示。 `ProductsBLL`類別將一樣運作，而不需要這些屬性，但將它們新增可讓您更輕鬆地使用 ObjectDataSource 的精靈中。

## <a name="adding-the-other-classes"></a>新增其他類別

使用`ProductsBLL`完整的類別，我們仍需要加入使用分類、 供應商和員工的類別。 請花一點時間來建立下列類別和方法使用上述範例中的概念：

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

值得注意的是一種方法是`SuppliersBLL`類別的`UpdateSupplierAddress`方法。 這個方法會提供更新就只是供應商的位址資訊的介面。 就內部而言，這個方法會在讀取`SupplierDataRow`所指定的物件`supplierID`(使用`GetSupplierBySupplierID`)、 設定其地址相關屬性，並接著呼叫向下`SupplierDataTable`的`Update`方法。 `UpdateSupplierAddress`方法如下所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

請參閱這篇文章的下載，我的 BLL 類別的完整實作。

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>步驟 2： 透過 BLL 類別存取具類型資料集

在第一個教學課程中，我們看到範例的直接輸入資料集以程式設計方式使用，但我們 BLL 類別加入，展示層應適用於 BLL 改為。 在 `AllProducts.aspx`從第一個教學課程中，範例`ProductsTableAdapter`來的產品清單繫結至 GridView，如下列程式碼所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

若要使用新的 BLL 類別，所有需要的變更是第一行程式碼取代`ProductsTableAdapter`物件`ProductBLL`物件：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

也可以使用 ObjectDataSource，以宣告方式 （與可以使用具類型資料集） 存取的 BLL 類別。 我們將在下列教學課程中討論更詳細地 ObjectDataSource。


[![在 GridView 中顯示產品清單](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**圖 3**: The List of Products GridView 中顯示 ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>步驟 3： 將欄位層級驗證新增至 DataRow 類別

欄位層級驗證會檢查與插入或更新時的商務物件的屬性值。 產品有些欄位層級驗證規則包括：

- `ProductName`欄位必須是 40 個字元長度等於或小於
- `QuantityPerUnit`欄位必須是 20 個字元長度等於或小於
- `ProductID`， `ProductName`，和`Discontinued`欄位是必要的但所有其他欄位為選擇性
- `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`欄位必須是大於或等於零

這些規則可以而且應該在資料庫層級來表示。 字元的限制`ProductName`並`QuantityPerUnit`欄位會擷取這些資料行中的資料類型`Products`資料表 (`nvarchar(40)`和`nvarchar(20)`分別)。 如果資料庫資料表資料行允許欄位是否必要和選擇性來表示`NULL`s。 四個[check 條件約束](https://msdn.microsoft.com/library/ms188258.aspx)存在，請確定只有大於或等於零的值可以發出到`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`資料行。

除了強制執行這些規則，在資料庫上的其應該也會強制執行資料集層級。 事實上，已會擷取欄位長度和值是必要或選擇性針對每個 DataTable DataColumns 組。 若要查看自動提供的現有欄位層級驗證，請移至 DataSet 設計工具，從 Datatable 的其中一個選取欄位，然後移至 屬性 視窗。 如 圖 4 所示`QuantityPerUnit`中的 DataColumn`ProductsDataTable`最大長度為 20 個字元，但允許`NULL`值。 如果我們嘗試設定`ProductsDataRow`的`QuantityPerUnit`屬性設為超過 20 個字元的字串值`ArgumentException`就會擲回。


[![DataColumn 提供基本欄位層級驗證](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**圖 4**: DataColumn 提供基本欄位層級驗證 ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-cs/_static/image8.png))


不幸的是，我們不能指定界限檢查，例如`UnitPrice`值必須大於或等於零，透過 [屬性] 視窗。 若要提供這種類型的欄位層級驗證我們要建立的 DataTable 的事件處理常式[ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx)事件。 中所述[前述教學課程](creating-a-data-access-layer-cs.md)，具類型資料集所建立的資料集、 Datatable 和 DataRow 物件可以透過使用部分類別擴充。 使用我們可以建立這項技術`ColumnChanging`事件處理常式`ProductsDataTable`類別。 建立中的類別著手`App_Code`名為資料夾`ProductsDataTable.ColumnChanging.cs`。


[![將新類別加入至 App_Code 資料夾](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**圖 5**： 將新類別加入`App_Code`資料夾 ([按一下以檢視完整大小的影像](creating-a-business-logic-layer-cs/_static/image11.png))


接下來，建立的事件處理常式`ColumnChanging`事件，可確保`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，並`ReorderLevel`資料行值 (如果不是`NULL`) 大於或等於零。 如果任何這類資料行超出範圍時，會擲回`ArgumentException`。

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>步驟 4： 加入自訂的商務規則的 BLL 類別

除了欄位層級驗證，可能牽涉到不同的實體或未表示的概念，在單一資料行層級，例如的高層級的自訂商務規則：

- 如果產品已經停售，其`UnitPrice`無法更新
- 員工所居住的國家必須依據居住的國家/地區他的經理相同
- 如果它是唯一的產品供應商所提供，無法停用產品

BLL 類別應該包含檢查，以確保遵循應用程式的商務規則。 這些檢查直接加入它們所套用的方法。

假設我們的商務規則指定的產品無法標示已停止時指定的供應商的唯一產品。 亦即，如果產品*X*是我們從供應商購買的唯一產品*Y*，我們無法將標記*X*如; 如果終止，不過，供應商*Y*有三項產品，提供我們*A*， *B*，和*C*、 然後我們可以在任何標示和所有的這些做為已停止。 奇數的商務規則，但是商務規則以及相關常識永遠不對齊的 ！

若要強制執行此商務規則`UpdateProducts`方法，我們會開始檢查是否`Discontinued`已設為`true`，因此，我們會呼叫`GetProductsBySupplierID`我們購買此產品的供應商來判斷多少的產品。 如果只有一項產品從這個供應商購買，我們會擲回`ApplicationException`。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>展示層中的驗證錯誤回應

從展示層呼叫 BLL 時我們可以決定是否要嘗試處理任何可能會引發，或將這些反昇至 ASP.NET 的例外狀況 (這將會引發`HttpApplication`的`Error`事件)。 若要處理的例外狀況，當以程式設計方式處理 BLL，我們可以使用[try...catch](https://msdn.microsoft.com/library/0yd65esw.aspx)區塊，如下列範例所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

我們會看到在未來的教學課程、 處理例外狀況所產生的 BLL 時使用的是資料 Web 控制項的插入、 更新或刪除資料可以直接在事件處理常式，而不需要將程式碼中的包裝中處理`try...catch`區塊。

## <a name="summary"></a>總結

設計完善的應用程式會建立不同的層，其中每一個封裝特定的角色。 在此文章系列的第一個教學課程中，我們建立使用具類型資料集，資料存取層在本教學課程我們建置商務邏輯層為一系列類別在我們的應用程式中`App_Code`呼叫我們的 DAL 的資料夾。 BLL 實作的欄位層級和企業層級的邏輯應用程式。 除了建立個別的 BLL，如同我們在本教學課程中，另一個選項是擴充 Tableadapter 的方法，透過使用部分類別。 不過，使用這項技術不允許我們覆寫現有的方法，也不會它區隔我們的 DAL 和我們的 BLL 為我們在本文中所採取的方法完全。

DAL 和完整的 BLL，我們已準備好開始在我們的展示層上項目。 在 [下一個教學課程](master-pages-and-site-navigation-cs.md)我們將簡短的 「 繞道 」，從資料存取的主題並定義整個教學課程使用的一致性頁面配置。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Liz Shulok、 Dennis Patterson、 Carlos Santos 和 Hilton giesenow 將。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-a-data-access-layer-cs.md)
> [下一頁](master-pages-and-site-navigation-cs.md)
