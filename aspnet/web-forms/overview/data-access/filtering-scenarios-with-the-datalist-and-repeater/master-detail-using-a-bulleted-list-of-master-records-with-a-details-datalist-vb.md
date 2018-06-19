---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: 主要/詳細資料的詳細資料 DataList (VB) 搭配使用的主要記錄項目符號清單 |Microsoft 文件
author: rick-anderson
description: 本教學課程中我們將壓縮上一個教學課程的兩個頁面主要/詳細資料的報表到單一頁面上，t 上顯示類別目錄名稱的項目符號清單...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d87dc7f4fb00e96d9eb2653e6fbc1efb8bb656c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889016"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>主要/詳細資料的詳細資料 DataList (VB) 搭配使用的主要記錄項目符號清單
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe)或[下載 PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> 本教學課程中我們將壓縮上一個教學課程的兩個頁面主要/詳細資料的報表到單一頁面上，螢幕的右側的所選類別目錄的產品螢幕的左邊顯示類別目錄名稱的項目符號清單。


## <a name="introduction"></a>簡介

在[前述教學課程](master-detail-filtering-acess-two-pages-datalist-vb.md)我們探討如何區隔主要/詳細資料報表，跨兩個頁面。 在主版頁面中，我們會使用中繼器控制項呈現類別的項目符號清單。 每個類別目錄名稱的超連結，按一下時，會採用詳細資料頁面，其中兩欄 DataList 示範這些產品的使用者屬於所選類別目錄。

本教學課程中我們將會壓縮兩頁教學課程到單一頁面上，螢幕左側的類別目錄名稱的項目符號清單顯示每個類別目錄名稱轉譯為 LinkButton。 按一下其中一個類別目錄名稱 LinkButtons 引發回傳，並將選取的類別的產品繫結至螢幕的右側的兩個資料行資料清單。 除了顯示每個類別 s 的名稱，在左側中繼器顯示有多少總產品會針對指定的類別目錄 （請參閱圖 1）。


[![在左邊顯示的類別名稱和產品的總計數](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**圖 1**: 類別名稱和產品的總計數字會顯示在左邊 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>步驟 1： 在螢幕的左邊顯示設有重複項

此教學課程中，我們必須將類別目錄項目符號清單，選取的類別的產品左邊會出現。 內容的網頁內可以使用標準的 HTML 項目的段落標記，不分行空格，放置`<table>`s，依此類推或透過階層式樣式表 (CSS) 技術。 所有教學課程到目前為止已用來定位 CSS 技術。 當我們建立巡覽使用者介面中我們主版頁面中[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中我們使用*絕對定位*，表示對於瀏覽的精確的像素位移清單和主要內容。 或者，CSS 可用來將一個項目透過另一個左右*浮動*。 我們可以有浮動中繼器 DataList 左邊會出現左邊的 選取的類別的產品項目符號清單的類別

開啟`CategoriesAndProducts.aspx`頁面`DataListRepeaterFiltering`資料夾並加入至頁面，中繼器和 DataList。 設定中繼器 s`ID`至`Categories`和 DataList s 至`CategoryProducts`。 請移至來源檢視，並放在自己的中繼器和 DataList 控制項`<div>`項目。 也就是說，括住在中繼器`<div>`項目第一次，然後在它自己 DataList`<div>`直接在中繼器之後的項目。 您的標記此時應該看起來如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

若要浮動 DataList 左邊中繼器，我們需要使用`float`CSS 樣式屬性，就像這樣：


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;`浮動第一個`<div>`左邊的第二個項目。 `width`和`padding-right`設定指出第一個`<div>`s`width`和之間加上多少填補`<div>`的元素內容和其右邊界。 如需有關在 CSS 中浮動項目簽出[Floatutorial](http://css.maxdesign.com.au/floatutorial/)。

而不是指定直接透過第一個樣式設定`<p>`元素 s`style`屬性，將改為建立新的 CSS 類別中的 s`Styles.css`名為`FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

然後我們可以取代`<div>`與`<div class="FloatLeft">`。

加入 CSS 類別及設定中的標記之後`CategoriesAndProducts.aspx`頁面，請移至設計工具。 您應該會看到中繼器浮點 DataList 左邊 （雖然右邊現在都只會出現為灰色方塊，因為我們尚未將其資料來源或範本發生）。


[![在 DataList 左側浮動中繼器](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**圖 2**: 中繼器浮動 DataList 左邊 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>步驟 2： 判斷每個類別目錄的產品數目

與周圍的標記完整中繼器和 DataList s，我們準備好要將類別資料繫結至中繼器控制項。 不過，如圖 1 中的類別目錄項目符號清單所示，除了每個分類的名稱我們也需要顯示類別目錄相關聯的產品數目。 若要存取此資訊，我們可以執行下列動作：

- **判斷 ASP.NET 頁面 s 程式碼後置類別的這項資訊。** 指定特定*`categoryID`* 我們可以呼叫來判斷關聯的產品數目`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 這個方法會傳回`ProductsDataTable`物件，其`Count`屬性會指出多少`ProductsRow`s 已存在，但這是所指定的產品數目*`categoryID`*。 我們可以建立`ItemDataBound`中繼器的繫結至中繼器，每個類別會呼叫事件處理常式`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法，並在輸出中包含其計數。
- **更新`CategoriesDataTable`具類型資料集包含`NumberOfProducts`資料行。** 我們可以更新`GetCategories()`方法中的`CategoriesDataTable`包含這項資訊或，或者，將`GetCategories()`為-並建立新`CategoriesDataTable`方法呼叫`GetCategoriesAndNumberOfProducts()`。

可讓 s 探索這兩個技術。 第一種方法會更容易實作方式，因為我們不 t 需要更新資料的存取層級;不過，它需要更多與資料庫通訊。 若要呼叫`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法中的`ItemDataBound`事件處理常式會將每個類別顯示中繼器中的額外資料庫呼叫。 使用這項技術有*N* + 1 的資料庫呼叫，其中*N*是中繼器中顯示的類別數目。 會使用第二個方法中，從每個類別目錄的相關資訊傳回產品計數`CategoriesBLL`類別 s `GetCategories()` (或`GetCategoriesAndNumberOfProducts()`) 方法，藉此會導致對資料庫的存取只一次。

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>判斷 ItemDataBound 事件處理常式中的產品數目

判斷中繼器 s 中的每個類別目錄的產品數目`ItemDataBound`事件處理常式不需要我們現有的資料存取層的任何修改。 可以直接在進行所有修改`CategoriesAndProducts.aspx`頁面。 啟動新增名為新 ObjectDataSource`CategoriesDataSource`透過中繼器 s 智慧標籤。 接下來，設定`CategoriesDataSource`ObjectDataSource，因此它會擷取資料的來源`CategoriesBLL`類別的`GetCategories()`方法。


[![設定使用 CategoriesBLL 類別的 GetCategories() 方法 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


每個項目`Categories`中繼器必須能按，並按一下時，會導致`CategoryProducts`DataList 以顯示所選取類別目錄的產品。 這可以透過建立每個類別的超連結，連結至這個相同的頁面 (`CategoriesAndProducts.aspx`)，但傳遞`CategoryID`透過方式一樣在上一個教學課程中我們所見的 querystring。 這種方法的優點是頁面，顯示特定類別的產品可以設為書籤，並由搜尋引擎編製索引。

或者，我們可以讓每個類別目錄的 LinkButton，這是我們會使用此教學課程中的方法。 LinkButton 會呈現為超連結使用者 s 瀏覽器中，但是按一下時，引發回傳;在回傳時，DataList 的 ObjectDataSource 需要重新整理以顯示屬於所選類別目錄的產品。 此教學課程中，使用超連結更具意義比使用 LinkButton;不過，可能有其他的情況下使用 LinkButton 更有利。 雖然超連結的方法是適用於此範例中，可讓 s 改為瀏覽使用 LinkButton。 如我們所見，則使用 LinkButton 會帶來一些挑戰，否則不會發生與超連結。 因此，在本教學課程中使用的 LinkButton 將反白顯示這些挑戰，並協助我們可能要使用的 LinkButton 而不是超連結的情況下提供解決方案。

> [!NOTE]
> 建議您使用重複本教學課程中使用超連結控制項或`<a>`LinkButton 這個項目。


下列標記顯示中繼器和 ObjectDataSource 的宣告式語法。 請注意中繼器的範本呈現每個項目為 LinkButton 分項清單：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> 在此教學課程中繼器必須啟用其檢視狀態 (請注意在省略`EnableViewState="False"`中繼器 s 宣告式語法)。 步驟 3 中我們將建立的事件處理常式的中繼器 s`ItemCommand`事件，我們要更新 DataList 的 ObjectDataSource 的`SelectParameters`集合。 中繼器的`ItemCommand`，不過，如果已停用檢視狀態贏了 t 引發。 請參閱[ASP.NET 問題的疑惑](http://scottonwriting.net/sowblog/posts/1263.aspx)和[其方案](http://scottonwriting.net/sowBlog/posts/1268.aspx)必須啟用檢視狀態的 s 中繼器的原因的詳細資訊`ItemCommand`引發事件。


LinkButton`ID`屬性值為`ViewCategory`並沒有其`Text`屬性集。 如果我們只是具有要顯示的類別名稱，我們會有文字屬性以宣告方式設定，透過資料繫結的語法，如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

不過，我們想要顯示這兩個類別的名稱*和*屬於該類別目錄的產品數目。 這項資訊可以擷取從中繼器 s`ItemDataBound`藉由呼叫事件處理常式`ProductBLL`類別 s`GetCategoriesByProductID(categoryID)`方法，以及決定的多少筆記錄會在產生傳回`ProductsDataTable`，如以下程式碼說明：


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

一開始我們先確定我們重新處理資料的項目 (一個其`ItemType`是`Item`或`AlternatingItem`)，然後參考`CategoriesRow`剛才已繫結至目前的執行個體`RepeaterItem`。 接下來，我們判斷這個分類的產品數目所建立的執行個體`ProductsBLL`類別，呼叫其`GetCategoriesByProductID(categoryID)`方法，以及判斷使用傳回的記錄數目`Count`屬性。 最後， `ViewCategory` ItemTemplate 中的 LinkButton 已參考和其`Text`屬性設定為*CategoryName* (*NumberOfProductsInCategory*)，其中*NumberOfProductsInCategory*格式化為零的小數位數的數字。

> [!NOTE]
> 或者，我們無法加入*格式函式*接受類別 s 的 ASP.NET 頁面 s 程式碼後置類別`CategoryName`和`CategoryID`值並傳回`CategoryName`數目與串連產品類別目錄中的 (由呼叫`GetCategoriesByProductID(categoryID)`方法)。 這種格式的函式的結果無法以宣告方式指派給 LinkButton 的 Text 屬性取代需要`ItemDataBound`事件處理常式。 請參閱[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)或[格式化 DataList 和中繼器基礎時的資料](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md)教學課程，如需有關格式的函式的使用。


加入此事件處理常式之後, 請花一點時間測試透過瀏覽器頁面。 請注意如何在項目符號清單中，顯示類別 s 的名稱和類別目錄相關聯的產品數目列出每個類別目錄 （請參閱圖 4）。


[![會顯示每個類別目錄名稱和數字的產品](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**圖 4**： 會顯示每個類別目錄名稱和數字的產品 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>更新`CategoriesDataTable`和`CategoriesTableAdapter`包含每個類別目錄的產品數目

而不是決定每個類別目錄做為它的產品數目 s 繫結至中繼器中，我們可以簡化這個程序，藉由調整`CategoriesDataTable`和`CategoriesTableAdapter`將此資訊包含在原生資料存取層中。 若要達成此目的，我們必須加入至新的資料行`CategoriesDataTable`保留相關聯的產品數目。 若要將新的資料行加入至 DataTable，開啟 輸入資料集 (`App_Code\DAL\Northwind.xsd`)，若要修改，資料表上按一下滑鼠右鍵，然後選擇 新增 / 資料行。 加入新的資料行， `CategoriesDataTable` （請參閱圖 5）。


[![將新的資料行加入至 CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**圖 5**： 加入新的資料行， `CategoriesDataSource` ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


這會新增新的資料行名為`Column1`，只要輸入不同的名稱，您可以變更。 重新命名這個新資料行`NumberOfProducts`。 接下來，我們需要設定此資料行的屬性。 按一下新的資料行，並移至 [屬性] 視窗。 變更資料行 s`DataType`屬性從`System.String`至`System.Int32`並設定`ReadOnly`屬性`True`，如圖 6 中所示。


![設定資料類型和新的資料行的 ReadOnly 屬性](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**圖 6**： 設定`DataType`和`ReadOnly`新資料行的屬性


雖然`CategoriesDataTable`現在具有`NumberOfProducts`資料行，其值不由設定的任何對應的 TableAdapter 的查詢。 我們可以更新`GetCategories()`方法以傳回此資訊，如果我們想要這類資訊傳回每次擷取類別目錄資訊。 如果，不過，我們只需要抓取在罕見情況下 （例如只針對本教學課程），類別目錄相關聯的產品數目，則我們可以保留`GetCategories()`為-並建立新的方法會傳回這項資訊。 可讓 s 使用這個第二種方法，建立名為的新方法`GetCategoriesAndNumberOfProducts()`。

若要加入新`GetCategoriesAndNumberOfProducts()`方法，以滑鼠右鍵按一下`CategoriesTableAdapter`選擇新的查詢。 這樣就會的向上 TableAdapter 查詢組態精靈中，哪些我們我們在先前的教學課程中多次使用。 這個方法，啟動精靈，指出此查詢使用特定 SQL 陳述式會傳回資料列。


[![建立使用特定 SQL 陳述式的方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**圖 7**： 建立方法使用特定 SQL 陳述式 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![SQL 陳述式會傳回資料列](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**圖 8**: SQL 陳述式的傳回資料列 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


下一個畫面中，精靈會提示的查詢來使用。 若要傳回每個類別目錄 s `CategoryID`， `CategoryName`，和`Description`欄位，該類別，與相關聯的產品數目以及使用下列`SELECT`陳述式：


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![指定要使用的查詢](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**圖 9**： 指定要使用的查詢 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


請注意，會計算與類別相關聯的產品數目的子查詢的別名為`NumberOfProducts`。 這個命名的比對會導致此相關聯的子查詢所傳回的值`CategoriesDataTable`s`NumberOfProducts`資料行。

輸入此查詢之後, 的最後一個步驟是選擇新方法的名稱。 使用`FillWithNumberOfProducts`和`GetCategoriesAndNumberOfProducts`填滿 DataTable 和傳回 DataTable 模式，分別。


[![新的 TableAdapter 的方法 FillWithNumberOfProducts 名稱和 GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**圖 10**： 命名新的 tableadapter 方法`FillWithNumberOfProducts`和`GetCategoriesAndNumberOfProducts`([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


此時資料存取層已經擴充成包含每個分類的產品數目。 因為所有我們展示層會路由傳送所有透過不同的商務邏輯層 DAL 呼叫我們需要加入對應`GetCategoriesAndNumberOfProducts`方法`CategoriesBLL`類別：


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

DAL 和 BLL 完成，我們重新準備好將這項資料給繫結`Categories`中繼器中的`CategoriesAndProducts.aspx`！ 如果您已建立 ObjectDataSource 判斷從中繼器的數字的產品的`ItemDataBound`事件處理常式區段中，刪除此 ObjectDataSource，並移除中繼器的`DataSourceID`屬性設定; 也 unwire中繼器 s`ItemDataBound`事件的事件處理常式，藉由移除`Handles Categories.OnItemDataBound`ASP.NET 程式碼後置類別中的語法。

中繼器中其原始狀態，加入名為新 ObjectDataSource`CategoriesDataSource`透過中繼器 s 智慧標籤。 設定用於 ObjectDataSource`CategoriesBLL`類別，而不需要使用它，但`GetCategories()`方法，這個方法有它使用`GetCategoriesAndNumberOfProducts()`改為 （請參閱圖 11）。


[![設定為使用 GetCategoriesAndNumberOfProducts 方法 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**圖 11**： 設定要使用 ObjectDataSource`GetCategoriesAndNumberOfProducts`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


接下來，更新`ItemTemplate`以便 LinkButton s`Text`屬性以宣告方式使用資料繫結語法所指派，並同時包含`CategoryName`和`NumberOfProducts`資料欄位。 中繼器的完整宣告式標記和`CategoriesDataSource`ObjectDataSource 遵循：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

藉由更新以包含 DAL 轉譯的輸出`NumberOfProducts`資料行是使用相同`ItemDataBound`事件處理常式方法 (指圖 4，請參閱螢幕擷取畫面，顯示類別目錄名稱和產品數目中繼器的)。

## <a name="step-3-displaying-the-selected-category-s-products"></a>步驟 3： 顯示選取的類別的產品

現在我們有`Categories`中繼器以及產品的數目的類別目錄的清單顯示每個類別中。 中繼器使用 LinkButton 每個類別目錄，按一下時，會導致回傳，此時點我們需要顯示所選取的類別目錄中的那些產品`CategoryProducts`DataList。

我們對向的一項挑戰是如何顯示這些產品選取類別目錄資料的清單。 在[主要/詳細說明使用詳細資料 DetailsView 的可選取的主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)無法選取教學課程中我們可了解如何建置 GridView 的資料列、 所選取的資料列 s 的詳細資料顯示在相同頁面上的 DetailsView 中。 GridView 的 ObjectDataSource 傳回使用的所有產品的相關資訊`ProductsBLL`s`GetProducts()`方法 DetailsView 的 ObjectDataSource 時擷取所選的產品的使用資訊`GetProductsByProductID(productID)`方法。 *`productID`* 參數值所提供以宣告方式將它與 GridView s 的值關聯`SelectedValue`屬性。 不幸的是，中繼器沒有`SelectedValue`屬性並不能做為參數來源。

> [!NOTE]
> 這是出現在中繼器中使用的 LinkButton 時這些挑戰。 我們使用超連結傳入`CategoryID`透過 querystring 相反地，我們無法用於該查詢字串欄位做為來源參數 s 的值。


我們會擔心缺乏之前`SelectedValue`中繼器的屬性，可讓 s 第一次將 DataList 繫結至 ObjectDataSource 並指定其`ItemTemplate`。

從 DataList s 智慧標籤，選擇 加入新的 ObjectDataSource 名為`CategoryProductsDataSource`並將它設定為使用`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 因為在本教學課程 DataList 提供唯讀介面，則可以自由設定下拉式清單中插入、 更新和刪除索引標籤，以 （無）。


[![設定為使用 ProductsBLL 類別的 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**圖 12**： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


因為`GetProductsByCategoryID(categoryID)`方法預期的輸入的參數 (*`categoryID`*)，設定資料來源精靈可讓我們來指定參數 s 的來源。 類別目錄列出 GridView 或 DataList 中，我們 d 設參數來源下拉式清單控制項和至 ControlID `ID` Web 控制項的資料。 不過，由於中繼器缺少`SelectedValue`屬性不能做為參數的來源。 如果您核取，您會看到 [ControlID] 下拉式清單只包含一個控制項`ID``CategoryProducts`、 `ID` datalist 的。

現在，設定參數來源下拉式清單為 None。 最後會得到以程式設計的方式指派此參數值時的 LinkButton 已按下中繼器中的類別。


[![請勿指定參數的來源 categoryID 參數](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**圖 13**： 未指定的參數來源*`categoryID`* 參數 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


完成設定資料來源精靈之後，Visual Studio 會自動產生 DataList 的`ItemTemplate`。 取代此預設`ItemTemplate`範本我們先前的教學課程中使用; 此外，設定 DataList 的`RepeatColumns`2 的屬性。 進行這些變更後 DataList 和其相關聯的 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

目前， `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* 永遠不會設定參數，以便檢視頁面時，會不顯示任何產品。 我們需要如何做會將此參數值，設定根據`CategoryID`中繼器中按下類別目錄。 這導入了兩個挑戰： 首先，請勿我們如何判斷當中繼器 s 中的 LinkButton`ItemTemplate`已按下; 和第二個，我們要如何判斷`CategoryID`的 LinkButton 已按下的對應類別目錄？

如同按鈕和 ImageButton 控制項 LinkButton 已`Click`事件和[`Command`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx)。 `Click`事件設計只需注意的 LinkButton 已按下。 有時候，不過，除了 LinkButton 已按下您會看到我們也需要一些額外的資訊傳遞至事件處理常式。 如果這種情況，LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx)和[ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx)屬性可以指派此額外資訊。 然後，當 LinkButton 已按下，其`Command`引發事件 (而不是其`Click`事件) 和事件處理常式傳遞的值`CommandName`和`CommandArgument`屬性。

當`Command`從中繼器，中繼器 s 中的範本內引發事件[`ItemCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx)引發並傳遞`CommandName`和`CommandArgument`按下後的 LinkButton 的值 (或按鈕或ImageButton)。 因此，若要判斷類別中繼器中 LinkButton 已按一下時，我們需要執行下列作業：

1. 設定`CommandName`屬性在中繼器的 LinkButton`ItemTemplate`為某個值 (我 ve 用 ListProducts)。 藉由設定此`CommandName`值 LinkButton 的`Command`LinkButton 已按下時，就會引發事件。
2. 設定 LinkButton s`CommandArgument`屬性設為值的目前項目的`CategoryID`。
3. 建立事件處理常式的中繼器的`ItemCommand`事件。 處理常式，在事件集`CategoryProductsDataSource`ObjectDataSource s`CategoryID`參數設為值的傳入`CommandArgument`。

下列`ItemTemplate`標記類別中繼器實作步驟 1 和 2。 請注意如何`CommandArgument`值會指派資料項目的`CategoryID`使用資料繫結語法：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

每當建立`ItemCommand`最好一定會先檢查傳入的事件處理常式`CommandName`值，因為*任何*`Command`引發事件*任何*按鈕、 LinkButton，或會導致在中繼器的 ImageButton`ItemCommand`引發事件。 雖然我們目前只能有一個這類 LinkButton 現在，未來我們 （或其他開發人員在我們的團隊） 可能會加入其他按鈕 Web 控制項中繼器，按一下時，會引發相同`ItemCommand`事件處理常式。 因此，它 s 最佳請務必確定您檢查`CommandName`屬性，才繼續執行您以程式設計方式的邏輯符合預期的值。

在確定傳入`CommandName`值等於 ListProducts，然後將此事件處理常式指派`CategoryProductsDataSource`ObjectDataSource s`CategoryID`參數設為值的傳入`CommandArgument`。 這項修改 ObjectDataSource s`SelectParameters`會自動讓資料重新本身繫結至資料來源，顯示新選取的分類的產品清單。


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

這些新增項目，與我們的教學課程已完成 ！ 請花一點時間瀏覽器中測試它。 圖 14 顯示螢幕時第一次瀏覽頁面。 因為類別尚未選取，則會不顯示任何產品。 按一下類別，例如產生，這些產品顯示產品類別目錄中的兩個資料行檢視 （請參閱圖 15）。


[![沒有產品會顯示當第一次瀏覽頁面](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**圖 14**: No 產品會顯示當第一次瀏覽頁面 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![產生類別目錄清單右邊的比對的產品](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**圖 15**： 按一下 產生類別目錄列出的比對產品右邊 ([按一下以檢視完整大小的影像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>總結

如我們所見在本教學課程和前一個，主要/詳細資料報表可以是分散在兩個頁面，或其中一個彙總。 在單一頁面上顯示的主要/詳細資料報表，不過，帶來一些挑戰如何最主要的版面配置和分頁上的詳細資訊記錄。 在*主要/詳細說明使用詳細資料 DetailsView 的可選取的主要 GridView*教學課程，我們必須顯示上述的主要記錄的詳細資訊記錄; 在本教學課程使用 CSS 的技術有要的主要記錄浮點數左邊的詳細資料。

顯示主要/詳細資料報表，以及我們也必須有機會探討如何擷取如何執行伺服器端邏輯 LinkButton （或按鈕或 ImageButton） 時按下從與每個類別目錄相關聯的產品數目中繼器。

本教學課程完成 DataList 與中繼器我們檢查的主要/詳細資料報表。 我們的教學課程的下一組將說明如何新增編輯和刪除 DataList 控制項的功能。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/)浮點 CSS 項目上的教學課程
- [CSS 位置](http://www.brainjar.com/css/positioning/)上定位項目 css 的詳細資訊
- [用來配置出內容與 HTML](http://www.w3schools.com/html/html_layout.asp)使用`<table>`s 和定位其他 HTML 項目

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Zack Jones。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](master-detail-filtering-acess-two-pages-datalist-vb.md)
