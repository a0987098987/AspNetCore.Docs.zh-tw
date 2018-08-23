---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: 使用具有詳細資料 DataList (VB) 的主要記錄項目符號清單的主要/詳細資料 |Microsoft Docs
author: rick-anderson
description: 在本教學課程會壓縮上一個教學課程的兩頁主要/詳細資料的報表到單一頁面上，t 上顯示的類別目錄名稱的項目符號清單...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a10d6e5f60efad1f88c5acc8371a24dbf8d2cb7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833203"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>使用具有詳細資料 DataList (VB) 的主要記錄項目符號清單的主要/詳細資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe)或[下載 PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> 在本教學課程會壓縮上一個教學課程的兩頁主要/詳細資料的報表到單一頁面上，顯示畫面和螢幕的右邊的所選的分類的產品的左側的類別名稱的項目符號清單。


## <a name="introduction"></a>簡介

在 [前述教學課程](master-detail-filtering-acess-two-pages-datalist-vb.md)我們探討了如何區隔跨兩個頁面的主版/詳細報告。 在主版頁面中，我們會使用重複項控制項呈現類別的項目符號清單。 每個類別目錄名稱是超連結，按一下時，會採用使用者的詳細資料頁面，其中兩個資料行 DataList 示範了這些產品屬於所選取的類別。

在本教學課程會壓縮兩頁教學課程到單一頁面上，顯示在畫面左側的類別名稱的項目符號清單呈現 LinkButton 為每個類別目錄名稱。 按一下其中一個類別目錄名稱 Linkbutton 引發回傳，並將選取的類別目錄產品繫結至畫面的右側兩個資料行 DataList。 除了顯示每個類別 s 的名稱，在左側的重複項顯示有多少總產品指定類別 （請參閱 圖 1）。


[![使用的類別名稱和產品的總計數字會顯示在左邊](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**圖 1**: 類別名稱和產品的總計數字會顯示在左側 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>步驟 1： 在螢幕的左邊顯示的重複項

在本教學課程中，我們需要有類別目錄項目符號清單，選取類別目錄產品的左邊會出現。 可以使用標準的 HTML 項目段落標記，不分行空格放置內容的 web 網頁內的位置`<table>`s，並依此類推或透過階層式樣式表 (CSS) 技術。 我們的教學課程的所有目前為止已用來定位 CSS 技巧。 當我們在我們的主版頁面中，會在建置巡覽使用者介面時[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，我們使用*絕對定位*，指出瀏覽的精準的像素位移清單和主要內容。 或者，使用 CSS，可以定位項目透過另一個左右*浮動*。 我們可以有類別目錄項目符號清單，顯示左邊的 選取類別目錄產品浮動至 DataList 左邊 Repeater

開啟`CategoriesAndProducts.aspx`頁面上，從`DataListRepeaterFiltering`資料夾和 Repeater 和 DataList 新增至頁面。 設定中繼器 s`ID`要`Categories`和 DataList s 至`CategoryProducts`。 移至來源檢視，並將其本身內 Repeater 和 DataList 控制項放`<div>`項目。 也就是括住在中繼器`<div>`項目第一次，然後在它自己 DataList`<div>`直接在中繼器之後的項目。 標記現在看起來應該如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

若要浮動至 DataList 左邊 Repeater，我們需要使用`float`CSS 樣式屬性，就像這樣：


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;`浮點數的第一個`<div>`左邊的第二個項目。 `width`並`padding-right`設定會指出第一個`<div>`s`width`之間加入多少填補和`<div>`元素的內容和其右邊界。 如需在 CSS 中浮動元素的詳細資訊請參閱[Floatutorial](http://css.maxdesign.com.au/floatutorial/)。

而不是指定的樣式設定，直接透過第一個`<p>`項目 s`style`屬性中，將改為建立新的 CSS 類別中的 s`Styles.css`名為`FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

然後我們可以把`<div>`與`<div class="FloatLeft">`。

加入 CSS 類別，並設定中的標記後`CategoriesAndProducts.aspx`頁面上，移至設計工具。 您應該會看到 Repeater （雖然權限現在都只會出現，因為我們尚未將其資料來源或範本的 ve 灰色方塊），浮點數到左邊的 DataList。


[![中繼器浮動至 DataList 左邊](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**圖 2**： 至 datalist 的左浮動的重複項 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>步驟 2： 判斷每個類別目錄的產品數目

與周圍的標記完成 Repeater 和 DataList s，我們準備好的類別目錄資料繫結至 Repeater 控制項。 不過，如 [圖 1] 中的類別目錄項目符號清單所示，除了每個類別目錄名稱，我們也需要顯示類別目錄相關聯的產品數目。 若要存取此資訊，我們可以：

- **判斷從 ASP.NET 頁面 s 程式碼後置類別的這項資訊。** 指定特定*`categoryID`* 我們可以呼叫來判斷相關聯的產品數目`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 這個方法會傳回`ProductsDataTable`物件，其`Count`屬性會指出多少`ProductsRow`s 已存在，但這是指定的產品數目*`categoryID`*。 我們可以建立`ItemDataBound`Repeater 的繫結至 Repeater，每個類別會呼叫事件處理常式`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法，並在輸出中包含其計數。
- **更新`CategoriesDataTable`輸入資料集中要包含`NumberOfProducts`資料行。** 我們可以接著更新`GetCategories()`方法中的`CategoriesDataTable`若要包含這項資訊或，或者，將`GetCategories()`做為-並建立新`CategoriesDataTable`方法呼叫`GetCategoriesAndNumberOfProducts()`。

可讓 s 探索這兩種技巧。 第一種方法是比較容易實作方式，因為我們不 t 需要更新資料的存取層級;不過，它需要多個資料庫通訊。 若要在呼叫`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法中的`ItemDataBound`事件處理常式會將每個類別顯示中繼器中額外的資料庫呼叫。 使用這項技術有*N* + 1 個資料庫呼叫，其中*N*是中繼器中顯示的類別數目。 會使用第二個方法時，從每個類別目錄的相關資訊傳回產品計數`CategoriesBLL`類別 s `GetCategories()` (或`GetCategoriesAndNumberOfProducts()`) 方法，進而導致只一次往返資料庫。

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>判斷 ItemDataBound 事件處理常式中的產品數目

判斷中繼器 s 中的每個類別目錄的產品數目`ItemDataBound`事件處理常式不需要我們現有的資料存取層的任何修改。 可以直接在進行所有修改`CategoriesAndProducts.aspx`頁面。 新增名為新 ObjectDataSource 開始`CategoriesDataSource`透過 Repeater s 智慧標籤。 接下來，設定`CategoriesDataSource`ObjectDataSource，因此它會擷取資料的來源`CategoriesBLL`類別的`GetCategories()`方法。


[![設定要使用 CategoriesBLL 類別的 GetCategories() 方法的 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


在每個項目`Categories`Repeater 必須可以點選，並按一下時，會導致`CategoryProducts`顯示選取之類別目錄的這些產品的 DataList。 這可藉由讓每個類別的超連結，連結回到這個相同的頁面 (`CategoriesAndProducts.aspx`)，但傳遞`CategoryID`透過查詢字串，如同我們在上一個教學課程中所見。 這種方法的優點是可以加上書籤頁面，顯示特定類別 s 的產品，並由搜尋引擎編製索引。

或者，我們可以讓每個類別目錄的 LinkButton，這是本教學課程中，我們將使用的方法。 LinkButton 會呈現為超連結使用者 s 瀏覽器中，但按一下時，會引發回傳;在回傳時，DataList 的 ObjectDataSource 需要重新整理以顯示屬於所選分類的產品。 本教學課程中，使用超連結會比使用 LinkButton;不過，可能有其他的情況下使用 LinkButton 更有幫助。 雖然超連結的方法是適用於此範例中，可讓 s 改為使用 LinkButton 進行探索。 如我們所見，使用了 LinkButton 導入了一些挑戰，否則不會發生的超連結。 因此，在本教學課程使用 LinkButton 將反白顯示這些挑戰，並協助提供這些的情況下我們可能會想要使用 LinkButton 而不是超連結的解決方案。

> [!NOTE]
> 建議您使用以重複執行本教學課程中使用超連結控制項或`<a>`代替 LinkButton 的項目。


下列標記顯示 Repeater 和 ObjectDataSource 的宣告式語法。 請注意 Repeater 的範本呈現 LinkButton 為每個項目的項目符號清單：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> 本教學課程中的重複項必須啟用其檢視狀態 (請注意省略的`EnableViewState="False"`Repeater s 宣告式語法)。 步驟 3 中我們將建立事件處理常式，如 Repeater s`ItemCommand`事件中我們將更新 DataList 的 ObjectDataSource 的`SelectParameters`集合。 Repeater 的`ItemCommand`，不過，如果已停用檢視狀態，贏得 t 引發。 請參閱[ASP.NET 問題的疑惑](http://scottonwriting.net/sowblog/posts/1263.aspx)並[其解決方案](http://scottonwriting.net/sowBlog/posts/1268.aspx)詳細了解為何必須啟用檢視狀態的 Repeater 的`ItemCommand`引發的事件。


使用 LinkButton`ID`屬性值`ViewCategory`並沒有其`Text`屬性集。 如果我們只要有想要顯示類別名稱，我們會設定 Text 屬性以宣告方式，透過資料繫結語法，就像這樣：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

不過，我們想要顯示這兩個類別目錄的名稱的地方*和*屬於該類別目錄的產品數目。 這項資訊可以擷取從 Repeater s`ItemDataBound`藉由呼叫的事件處理常式`ProductBLL`類別 s`GetCategoriesByProductID(categoryID)`方法，並判斷在產生傳回多少筆記錄`ProductsDataTable`，如以下程式碼說明：


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

一開始我們先確定我們重新處理資料的項目 (一個其`ItemType`是`Item`或`AlternatingItem`)，然後參考`CategoriesRow`只是已繫結至目前的執行個體`RepeaterItem`。 接下來，我們判斷此分類的產品數目所建立的執行個體`ProductsBLL`類別，呼叫其`GetCategoriesByProductID(categoryID)`方法，並決定使用傳回的記錄數目`Count`屬性。 最後， `ViewCategory` itemtemplate 的 LinkButton 是參考及其`Text`屬性設定為*CategoryName* (*NumberOfProductsInCategory*)，其中*NumberOfProductsInCategory*會格式化為零的小數位數的數字。

> [!NOTE]
> 或者，我們可以新增*格式化函式*接受類別 s 為 ASP.NET 網頁 s 程式碼後置類別`CategoryName`並`CategoryID`值並傳回`CategoryName`串連在一起的數目產品類別目錄中的 (由呼叫`GetCategoriesByProductID(categoryID)`方法)。 這種格式的函式的結果無法以宣告方式指派給 LinkButton 的 Text 屬性取代需要`ItemDataBound`事件處理常式。 請參閱[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)或是[格式化 DataList 和 Repeater 時資料](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md)如需有關使用格式化的函式的教學課程。


加入這個事件處理常式之後, 請花一點時間測試透過瀏覽器頁面。 請注意每個類別目錄會列在項目符號清單中，顯示分類的名稱與類別相關聯的產品數目的方式 （請參閱 圖 4）。


[![會顯示每個類別目錄名稱和數字的產品](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**圖 4**： 顯示每個類別目錄名稱和數字的產品 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>更新`CategoriesDataTable`和`CategoriesTableAdapter`来包含的產品數目，每個類別目錄

而不是決定每個類別目錄做為它的產品數目 s 繫結至 Repeater，我們可以簡化這個程序，藉由調整`CategoriesDataTable`和`CategoriesTableAdapter`中將此資訊包含在原生資料存取層。 若要達到此目的，我們必須加入至新的資料行`CategoriesDataTable`保留相關聯的產品數目。 若要加入新的資料行至 DataTable，開啟 輸入資料集 (`App_Code\DAL\Northwind.xsd`)，以滑鼠右鍵按一下要修改，DataTable，然後選擇 新增 / 資料行。 加入新的資料行，以`CategoriesDataTable`（請參閱 [圖 5]）。


[![將新的資料行新增至 CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**圖 5**： 加入新的資料行，以`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


這會新增新的資料行，名為`Column1`，您可以變更輸入其他名稱。 重新命名此新資料行`NumberOfProducts`。 接下來，我們需要設定此資料行的屬性。 按一下新的資料行，然後移至 [屬性] 視窗。 變更資料行 s`DataType`屬性從`System.String`要`System.Int32`，並設定`ReadOnly`屬性設`True`，如 [圖 6] 所示。


![設定資料類型和新的資料行的唯讀屬性](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**圖 6**： 設定`DataType`和`ReadOnly`新的資料行的屬性


雖然`CategoriesDataTable`現在有`NumberOfProducts`資料行，其值不由設定的任何對應的 TableAdapter 的查詢。 我們可以更新`GetCategories()`方法來傳回這項資訊，如果我們想要這類資訊傳回，每次擷取類別目錄資訊時。 如果，不過，我們只需要擷取相關聯的產品，在罕見情況下 （例如，只用於本教學課程），類別目錄數目，則我們可以把`GetCategories()`作為-並建立新的方法會傳回這項資訊。 可讓 s 使用此第二種方法，建立名為的新方法`GetCategoriesAndNumberOfProducts()`。

若要新增這`GetCategoriesAndNumberOfProducts()`方法，以滑鼠右鍵按一下`CategoriesTableAdapter`，然後選擇 新增查詢。 這會的啟動 TableAdapter 查詢組態精靈 中，這點我們發生許多次上一個教學課程中使用。 針對這個方法，請藉由指出此查詢使用特定 SQL 陳述式會傳回資料列來啟動精靈。


[![建立使用特定 SQL 陳述式的方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**圖 7**： 建立方法使用特定 SQL 陳述式 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![SQL 陳述式會傳回資料列](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**圖 8**: SQL 陳述式傳回資料列 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


精靈的下一個畫面會提示我們輸入的查詢來使用。 若要傳回每個類別 s `CategoryID`， `CategoryName`，並`Description`使用下列項目欄位，以及在類別中，相關聯的產品數目`SELECT`陳述式：


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![指定要使用的查詢](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**圖 9**： 指定要使用的查詢 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


請注意 計算與分類相關聯的產品數目的子查詢的別名為`NumberOfProducts`。 此命名的相符項目會導致此相關聯的子查詢所傳回的值`CategoriesDataTable`s`NumberOfProducts`資料行。

輸入此查詢之後, 的最後一個步驟是選擇新方法的名稱。 使用`FillWithNumberOfProducts`和`GetCategoriesAndNumberOfProducts`填滿的 DataTable 和傳回 DataTable 模式，分別。


[![新的 TableAdapter 的方法 FillWithNumberOfProducts 名稱和 GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**圖 10**： 命名新的 tableadapter 方法`FillWithNumberOfProducts`並`GetCategoriesAndNumberOfProducts`([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


此時資料存取層已經擴充成包含每個分類的產品數目。 因為我們所有的展示層所有將來電路由到不同的商務邏輯層透過 DAL 我們需要加入相對應`GetCategoriesAndNumberOfProducts`方法，以`CategoriesBLL`類別：


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

我們重新就緒的 DAL 和完整的 BLL，此資料要繫結`Categories`中的重複項`CategoriesAndProducts.aspx`！ 如果您已建立 ObjectDataSource 從判斷重複項的數字的中的產品`ItemDataBound`事件處理常式 區段中，刪除這個 ObjectDataSource，並移除重複項的`DataSourceID`屬性也設定; unwireRepeater s`ItemDataBound`事件的事件處理常式，藉由移除`Handles Categories.OnItemDataBound`ASP.NET 程式碼後置類別中的語法。

傳回在其原始狀態中重複項，加上名為新 ObjectDataSource`CategoriesDataSource`透過 Repeater s 智慧標籤。 設定要使用 ObjectDataSource`CategoriesBLL`類別，而不需要使用它，但`GetCategories()`方法中，有它使用`GetCategoriesAndNumberOfProducts()`改為 （請參閱 圖 11）。


[![設定為使用 GetCategoriesAndNumberOfProducts 方法的 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**圖 11**： 設定要使用 ObjectDataSource`GetCategoriesAndNumberOfProducts`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


接下來，更新`ItemTemplate`以便 LinkButton s`Text`屬性以宣告方式使用資料繫結語法所指派，且同時包含`CategoryName`和`NumberOfProducts`資料欄位。 中繼器的完整宣告式標記和`CategoriesDataSource`ObjectDataSource 遵循：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

藉由更新以包含 DAL 轉譯的輸出`NumberOfProducts`資料行是使用相同`ItemDataBound`事件處理常式方法 (回頭查看畫面的 圖 4 顯示類別目錄名稱和產品數目中繼器的螢幕擷取)。

## <a name="step-3-displaying-the-selected-category-s-products"></a>步驟 3： 顯示所選類別目錄產品

現在我們有`Categories`顯示以及產品的數目的類別目錄的清單，每個類別中的重複項。 中繼器每個類別目錄，按一下時，會導致回傳，在這點我們使用了 LinkButton 顯示選取之類別目錄中的這些產品需要`CategoryProducts`DataList。

我們面臨的其中一項挑戰是如何顯示這些產品所選類別的 DataList。 在 [主要/詳細說明使用具有詳細資料 DetailsView 可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)無法選取我們了解如何建置 GridView 其資料列的教學課程、 所選取的資料列 s 的詳細資料顯示在相同的頁面上的 DetailsView。 GridView ObjectDataSource 傳回使用的所有產品的相關資訊`ProductsBLL`s`GetProducts()`方法並 DetailsView 的 ObjectDataSource 擷取所選的產品的使用資訊`GetProductsByProductID(productID)`方法。 *`productID`* 參數值以宣告方式將它與 GridView s 的值關聯提供`SelectedValue`屬性。 不幸的是，重複項並沒有`SelectedValue`屬性並不能做為參數的來源。

> [!NOTE]
> 這是其中一項中使用 LinkButton 時，會出現這些挑戰。 我們使用超連結來傳入`CategoryID`透過查詢字串相反的我們可以使用該查詢字串欄位做為來源參數 s 的值。


我們會擔心缺乏之前`SelectedValue`的重複項的屬性，可讓 s 先繫結至 ObjectDataSource 的 DataList，並指定其`ItemTemplate`。

從 DataList s 智慧標籤，選擇 新增名為新 ObjectDataSource`CategoryProductsDataSource`並將它設定為使用`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 因為在本教學課程 DataList 提供唯讀介面，放心地設定下拉式清單中的插入、 更新和刪除 （無） 索引標籤。


[![設定為使用 ProductsBLL 類別的 GetProductsByCategoryID(categoryID) 方法的 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**圖 12**： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


由於`GetProductsByCategoryID(categoryID)`方法所預期的輸入的參數 (*`categoryID`*)，設定資料來源精靈可讓我們指定的參數 s 的來源。 已分類已列在 GridView 或 DataList，d 我們設定參數來源下拉式清單控制項和到 ControlID `ID` Web 控制項的資料。 不過，由於 Repeater 缺少`SelectedValue`屬性不能做為參數的來源。 如果您核取，您會發現 [ControlID] 下拉式清單只包含一個控制項`ID``CategoryProducts`，則`ID`的 DataList。

現在，設定參數來源下拉式清單為 None。 就會出現以程式設計方式將指派此參數值，當按一下 LinkButton 中繼器中的類別。


[![請勿指定 categoryID 參數參數來源](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**圖 13**： 未指定參數來源*`categoryID`* 參數 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


完成設定資料來源精靈之後，Visual Studio 會自動產生 DataList 的`ItemTemplate`。 取代此預設值`ItemTemplate`範本與我們先前的教學課程中使用; 此外，設定 DataList 的`RepeatColumns`屬性設為 2。 進行這些變更後您 DataList 和其相關聯的 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

目前， `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* 永遠不會設定參數，以便檢視頁面時，會不顯示任何產品。 我們要做為將此參數值，設定根據`CategoryID`中繼器中的已按下 類別目錄。 這引進了兩個挑戰： 首先，執行我們如何判斷當中繼器 s 中的 LinkButton`ItemTemplate`已按下; 和第二個，我們要如何判斷`CategoryID`其 LinkButton 已按下的對應類別目錄？

例如 「 按鈕 」 和 「 ImageButton 控制項 LinkButton 已`Click`事件和[`Command`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx)。 `Click`活動的設計僅僅 LinkButton 已按下。 有些時候，不過，除了 LinkButton 已按下您會看到我們也需要將一些額外的資訊傳遞至事件處理常式。 如果此情況下，使用的 LinkButton 秒[ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx)並[ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx)屬性可指定此額外資訊。 接下來，當按一下 LinkButton，其`Command`引發事件 (而不是其`Click`事件) 和事件處理常式傳遞的值`CommandName`和`CommandArgument`屬性。

當`Command`事件從 Repeater，Repeater s 中的範本內引發[`ItemCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx)就會引發，並傳遞`CommandName`和`CommandArgument`按一下 LinkButton 的值 (或按鈕或ImageButton)。 因此，若要判斷 LinkButton 中繼器中的類別時已按下，我們需要執行下列作業：

1. 設定`CommandName`屬性中 Repeater 的 LinkButton`ItemTemplate`為某個值 (我使用 ListProducts)。 藉由設定這`CommandName`的值，LinkButton 的`Command`LinkButton 已按下時，就會引發事件。
2. 設定使用的 LinkButton 秒`CommandArgument`屬性設為值的目前項目的`CategoryID`。
3. 建立事件處理常式，如 Repeater 的`ItemCommand`事件。 在事件處理常式中，設定`CategoryProductsDataSource`ObjectDataSource s`CategoryID`參數設為值的傳入`CommandArgument`。

下列`ItemTemplate`類別重複項的標記實作步驟 1 和 2。 請注意如何`CommandArgument`值會指派資料項目的`CategoryID`使用資料繫結語法：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

每當建立`ItemCommand`最好一律先檢查傳入的事件處理常式`CommandName`值，因為*任何*`Command`引發事件*任何*按鈕、 LinkButton，或ImageButton Repeater 內會導致`ItemCommand`引發的事件。 雖然我們目前只有一個這類 LinkButton 現在，未來我們 （或另一個開發人員在我們的團隊） 可能會額外的按鈕 Web 將控制項加入至 Repeater，按一下時，會引發相同`ItemCommand`事件處理常式。 因此，它最好隨時確定您檢查`CommandName`屬性，然後如果個比對預期的值只繼續您的程式設計邏輯。

在確定傳入`CommandName`值等於 ListProducts，然後將指派的事件處理常式`CategoryProductsDataSource`ObjectDataSource s`CategoryID`參數設為值的傳入`CommandArgument`。 這項修改為 ObjectDataSource 的`SelectParameters`會自動導致 DataList 重新本身繫結至資料來源，顯示新選取的分類的產品。


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

使用這些新增項目，在本教學課程已完成 ！ 請花一點時間瀏覽器中測試它。 第一次造訪網頁時，圖 14 顯示的畫面。 因為類別目錄尚未選取，則會不顯示任何產品。 按一下類別，產生，例如產品類別目錄，在兩個資料行] 檢視中顯示這些產品 （請參閱 [圖 15）。


[![沒有產品會顯示當第一次瀏覽頁面](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**圖 14**： 否產品會顯示當第一次瀏覽頁面 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![按一下 產生類別目錄清單右邊的比對的產品](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**圖 15**： 按一下 [產生] 類別列出比對產品向右 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>總結

如我們所見本教學課程和上例中，主版/詳細報告可以分散到兩個頁面，或其中一個彙總。 在單一頁面上顯示主版/詳細資料報表，不過，導入了一些有關的挑戰最主要的版面配置和分頁上的詳細資訊記錄。 在 *主要/詳細說明使用具有詳細資料 DetailsView 可選取主要 GridView*教學課程，我們必須顯示上述的主要記錄的詳細資訊記錄; 在本教學課程使用 CSS 技巧有要的主要記錄浮點數詳細資料的左方。

以及顯示主版/詳細報告，我們也有機會將探討如何擷取的方式來執行伺服器端邏輯的 LinkButton （或按鈕或 ImageButton） 按一下從與每個類別目錄相關聯的產品數目重複項。

本教學課程中完成主版/詳細報告我們的檢查使用 DataList 與重複項。 我們的下一個教學課程一組將說明如何加入編輯和刪除 DataList 控制項的功能。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/)浮動 CSS 項目，使用 CSS 的教學課程
- [CSS 定位](http://www.brainjar.com/css/positioning/)上定位項目使用 CSS 的詳細資訊
- [配置出內容與 HTML](http://www.w3schools.com/html/html_layout.asp)使用`<table>`s 和其他的 HTML 項目來定位

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](master-detail-filtering-acess-two-pages-datalist-vb.md)
