---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: 巢狀的資料 Web 控制項 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們將探討如何使用 Repeater 會放在另一個重複項中。 範例將說明如何將內部的 Repeater 填入這兩個 d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 032321b5cf5323058c114e652512854f9866d447
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833209"
---
<a name="nested-data-web-controls-c"></a>巢狀的資料 Web 控制項 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe)或[下載 PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> 在本教學課程，我們將探討如何使用 Repeater 會放在另一個重複項中。 範例將說明如何以宣告方式和以程式設計方式填入內部的重複項。


## <a name="introduction"></a>簡介

除了靜態 HTML 和資料繫結語法，範本也可以包含 Web 控制項和使用者控制項。 這些 Web 控制項可以有其屬性指派透過宣告式資料繫結語法，或在適當的伺服器端事件處理常式中可以透過程式設計方式存取。

藉由內嵌在範本內的控制項，可以自訂並改善了的外觀和使用者體驗。 例如，在[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程中，我們了解如何加入以自訂 GridView 的顯示日曆控制項中顯示員工的雇用日期為 TemplateField; 在[新增驗證控制項，以編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)並[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程中，我們可了解如何自訂編輯和插入介面藉由新增驗證控制項、 文字方塊、 dropdownlist 進行和其他 Web 控制項。

範本也可以包含其他資料 Web 控制項。 也就是我們可以包含另一個 DataList （或重複項或 GridView 或 DetailsView 等等） 在其樣板內 DataList。 這類介面的挑戰將適當的資料繫結至 Web 控制項的內部資料。 另外還有幾個不同的方法，範圍從使用程式設計的 ObjectDataSource 的宣告式選項。

在本教學課程，我們將探討如何使用 Repeater 會放在另一個重複項中。 外部 Repeater 會包含在資料庫中，每個類別的項目顯示的類別目錄的名稱和描述。 每個類別目錄項目 s 內部 Repeater 會顯示屬於該類別的每個產品的資訊 （請參閱 圖 1） 中的項目符號清單。 我們的範例將說明如何以宣告方式和以程式設計方式填入內部的重複項。


[![列出每個類別，以及其產品，](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**圖 1**： 列出每個類別，以及其產品中，([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>步驟 1： 建立類別目錄清單

當建置頁面，使用巢狀資料 Web 控制項時，我覺得很有幫助設計、 建立和第一次，而不需甚至擔心內部的巢狀控制測試最外層資料 Web 控制項。 因此，可讓 s 開始逐步解說將重複項新增至頁面，其中列出的名稱和描述每個類別目錄的必要步驟。

首先開啟`NestedControls.aspx`頁面中`DataListRepeaterBasics`資料夾並將重複項控制項新增至頁面上，設定其`ID`屬性設`CategoryList`。 從 Repeater s 智慧標籤，選擇 建立新的 ObjectDataSource 名為`CategoriesDataSource`。


[![命名新的 ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**圖 2**： 命名新 ObjectDataSource `CategoriesDataSource` ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image6.png))


因此，它會提取資料的來源設定 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法。


[![設定為使用 CategoriesBLL 類別的 GetCategories 方法的 ObjectDataSource](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories`方法 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image9.png))


若要指定 Repeater 的範本內容我們需要移至來源檢視，然後手動輸入的宣告式語法。 新增`ItemTemplate`顯示中的類別目錄的名稱`<h4>`項目和類別目錄的描述中的段落項目 (`<p>`)。 此外，可讓 s 使用水平尺規來分隔每個類別目錄 (`<hr>`)。 進行這些變更後您的頁面應該包含 Repeater 和與下例類似的 ObjectDataSource 的宣告式語法：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

[圖 4] 顯示我們透過瀏覽器檢視時的進度。


[![每個類別名稱和描述列出，以分隔的水平尺規](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**圖 4**： 每個類別目錄名稱和描述列出，以水平尺規分隔 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>步驟 2： 加入巢狀的產品 Repeater

列出完整的類別，與我們的下一個工作是加入至 Repeater `CategoryList` s `ItemTemplate` ，顯示這些產品屬於適當的類別目錄的相關資訊。 有數種方式，我們可以擷取此內部的重複項，其中兩個我們將探討短時間內的資料。 現在，讓 s 只建立產品 Repeater 內`CategoryList`Repeater 的`ItemTemplate`。 具體來說，可讓 s 有重複項顯示每個產品項目符號清單中的每個清單項目包括產品的名稱和價格的產品。

若要建立我們需要以手動方式輸入的內部 Repeater s 宣告式語法和範本到這個 Repeater `CategoryList` s `ItemTemplate`。 將下列標記內加入`CategoryList`Repeater 的`ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>步驟 3： 繫結 ProductsByCategoryList Repeater 的特定類別的產品

如果您在此時造訪透過瀏覽器頁面，您的畫面看起來如 圖 4 所示相同因為我們尚未將任何資料繫結至 Repeater ve。 有幾種方式，我們可以用來擷取適當的產品記錄，並將它們繫結至 Repeater，比其他一些更有效率。 主要的挑戰上一步取得適當的產品，為指定的類別目錄。

要繫結至內部的 Repeater 控制項的資料可以是透過以宣告方式，在 ObjectDataSource `CategoryList` Repeater 的`ItemTemplate`，或以程式設計的方式，從 ASP.NET 頁面 s 程式碼後置頁面。 同樣地，這項資料可以繫結到內部的重複項是以宣告方式為透過內部 Repeater s`DataSourceID`屬性或透過宣告式資料繫結語法或以程式設計方式參考內部的重複項中`CategoryList`Repeater s`ItemDataBound`事件處理常式，以程式設計方式設定其`DataSource`屬性，並呼叫其`DataBind()`方法。 可讓探索每一種方式的 s。

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>存取的資料使用 ObjectDataSource 控制項以宣告方式和`ItemDataBound`事件處理常式

因為我們使用廣泛地在本教學課程系列，最自然的選擇，以存取資料，如這個範例是要堅持使用 ObjectDataSource ObjectDataSource。 `ProductsBLL`類別具有`GetProductsByCategoryID(categoryID)`方法會傳回屬於指定這些產品的相關資訊*`categoryID`*。 因此，我們可以在其中新增到 ObjectDataSource `CategoryList` Repeater 的`ItemTemplate`並將它設定為從這個類別的方法存取其資料。

不幸的是，重複項不允許其範本，因此我們需要以手動方式加入這個 ObjectDataSource 控制項宣告式語法，透過 [設計] 檢視進行編輯。 下列語法會顯示`CategoryList`Repeater s`ItemTemplate`之後新增此新的 ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

使用 ObjectDataSource 方法時，我們需要設定`ProductsByCategoryList`Repeater s`DataSourceID`屬性設`ID`的 objectdatasource (`ProductsByCategoryDataSource`)。 另請注意，我們的 ObjectDataSource 已`<asp:Parameter>`項目，指定*`categoryID`* 值，會傳遞至`GetProductsByCategoryID(categoryID)`方法。 但是，我們該如何指定此值？ 在理想情況下，d 我們可以只設定`DefaultValue`屬性`<asp:Parameter>`項目使用資料繫結語法，就像這樣：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

不幸的是，資料繫結語法只適用於在控制項具有`DataBinding`事件。 `Parameter`類別缺少這類事件，且因此上述語法是不合法的而且將會導致執行階段錯誤。

若要設定此值，我們需要建立的事件處理常式`CategoryList`Repeater 的`ItemDataBound`事件。 請記得，`ItemDataBound`事件引發一次每個項目繫結至 Repeater。 因此，針對外部 Repeater 就會引發這個事件每次我們可以指派目前`CategoryID`值加入`ProductsByCategoryDataSource`ObjectDataSource 的`CategoryID`參數。

建立事件處理常式`CategoryList`Repeater 的`ItemDataBound`為下列程式碼的事件：


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

這個事件處理常式一開始會確保我們重新處理資料項目而不是標頭、 頁尾或分隔符號的項目。 接下來，我們會參考實際`CategoriesRow`剛才已繫結至目前的執行個體`RepeaterItem`。 最後，我們會參考內的 ObjectDataSource`ItemTemplate`並指派其`CategoryID`參數值以`CategoryID`目前的`RepeaterItem`。

與這個事件處理常式中，`ProductsByCategoryList`在每個重複項`RepeaterItem`繫結至在這些產品`RepeaterItem`的類別。 [圖 5] 顯示產生的輸出的螢幕擷取畫面。


[![外部 Repeater 列出每個類別目錄中;內部的其中一個列出的產品類別目錄](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**圖 5**: 外部 Repeater 列出每個類別目錄; 內部一個列出的產品類別目錄 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>以程式設計方式存取的產品類別目錄資料

而不是使用 ObjectDataSource 擷取目前的分類的產品，我們可以在我們的 ASP.NET 頁面 s 程式碼後置類別中建立方法 (或是在`App_Code`資料夾或個別的類別庫專案中)，傳回適當的集合產品時傳入`CategoryID`。 假設我們有這種方法，在我們的 ASP.NET 頁面 s 程式碼後置類別中，而名為`GetProductsInCategory(categoryID)`。 使用此方法就地我們可使用下列的宣告式語法內部 Repeater 繫結目前分類的產品：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Repeater s`DataSource`屬性會使用資料繫結語法來表示其資料是來自`GetProductsInCategory(categoryID)`方法。 由於`Eval("CategoryID")`傳回值的型別`Object`，我們將物件轉換成`Integer`，然後將傳遞到`GetProductsInCategory(categoryID)`方法。 請注意，`CategoryID`存取透過資料繫結語法如下`CategoryID`中*外部*Repeater (`CategoryList`)，則該 s 繫結中的記錄`Categories`資料表。 因此，我們知道`CategoryID`不可為資料庫`NULL`值，這就是為什麼我們可以盲目地轉型`Eval`方法，而不檢查是否我們重新處理`DBNull`。

使用此方法時，我們需要建立`GetProductsInCategory(categoryID)`方法並擷取適當的產品提供提供一組*`categoryID`*。 我們可以這樣做只是傳回`ProductsDataTable`所傳回`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 可讓建立`GetProductsInCategory(categoryID)`方法的程式碼後置類別中我們`NestedControls.aspx`頁面。 會使用下列程式碼進行存取：


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

這個方法只會建立的執行個體`ProductsBLL`方法，並傳回的結果`GetProductsByCategoryID(categoryID)`方法。 請注意必須標記的方法`Public`或是`Protected`; 如果方法已標記為`Private`，它將無法存取，從 ASP.NET 頁面 s 的宣告式標記。

進行這些變更来使用這個新的技術之後，請花一點時間檢視透過瀏覽器頁面。 使用 ObjectDataSource 時，輸出應該是相同的輸出和`ItemDataBound`事件處理常式方法 （請參閱上一步查看螢幕擷取畫面的 圖 5）。

> [!NOTE]
> 它看起來好像建立讓`GetProductsInCategory(categoryID)`ASP.NET 頁面 s 程式碼後置類別中的方法。 畢竟，這個方法只會建立的執行個體`ProductsBLL`類別，並傳回的結果，其`GetProductsByCategoryID(categoryID)`方法。 為什麼不只呼叫這個方法直接從資料繫結語法，在內部的重複項，例如： `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`？ 此語法贏得 t 使用我們的目前實作雖然`ProductsBLL`類別 (由於`GetProductsByCategoryID(categoryID)`方法是執行個體方法)，您可以修改`ProductsBLL`包含靜態`GetProductsByCategoryID(categoryID)`方法或有包含靜態類別`Instance()`方法傳回的新執行個體`ProductsBLL`類別。


雖然這類修改就不需要`GetProductsInCategory(categoryID)`ASP.NET 頁面 s 程式碼後置類別中的方法，程式碼後置類別方法會提供更大的彈性來使用資料擷取，很快就如稍後所示。

## <a name="retrieving-all-of-the-product-information-at-once"></a>擷取所有的產品資訊一次

兩個的舊技術我們 ve 檢查抓取這些產品目前的類別，藉由呼叫`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 (第一種方法有可能是透過 ObjectDataSource，透過第二個`GetProductsInCategory(categoryID)`方法程式碼後置類別）。 這個方法會叫用，商務邏輯層呼叫到資料存取層，每次查詢傳回的資料列的 SQL 陳述式的資料庫`Products`資料表的`CategoryID`欄位符合所提供的輸入的參數。

給定*N*系統中的類別，這種方法網路*N* + 1 呼叫資料庫有一個資料庫查詢，以取得所有的類別，然後*N*呼叫以取得產品每個類別所特有。 我們可以不過，擷取所有所需的資料，只有兩個資料庫來取得所有類別，以取得所有產品的另一種呼叫一次呼叫中。 一旦我們擁有的所有產品，我們可以因此篩選這些產品只比對目前的產品`CategoryID`繫結至該類別 s 內部的重複項。

若要提供這項功能，我們只需要進行稍微修改一下`GetProductsInCategory(categoryID)`我們 ASP.NET 頁面 s 程式碼後置類別中的方法。 而不會盲目地傳回的結果`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法中，我們可以改為第一次存取*所有*的產品 (如果它們尚未 t 已已經存取)，然後傳回只在已篩選的檢視產品以傳入的`CategoryID`。


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

請注意頁面層級變數新增`allProducts`。 這個保留的所有產品的相關資訊，而且會填入第一次`GetProductsInCategory(categoryID)`叫用方法。 在確定`allProducts`，已建立並填入物件，只將其資料列，方法會篩選 DataTable 的結果`CategoryID`符合指定`CategoryID`存取。 這種方法可減少從存取資料庫的次數*N* + 1 到兩個。

這項增強功能不會引進任何變更要呈現的標記的頁面上，也不會讓另一種方法比傳回較少的記錄。 它只會減少資料庫呼叫的數目。

> [!NOTE]
> 其中一個直覺的方式可能原因，減少資料庫存取數目會要改善效能。 不過，這可能不是如此。 如果您有大量產品其`CategoryID`已`NULL`，如範例中，然後呼叫`GetProducts`方法會傳回永遠不會顯示的產品數目。 此外，傳回所有的產品可以是浪費資源如果您只顯示類別，這可能是，如果您已實作分頁的子集。


如往常，談到分析兩種技術的效能，只有 surefire 的量值就是執行受到控制的測試，針對您的應用程式 s 常見案例量身打造。

## <a name="summary"></a>總結

在本教學課程中我們了解如何建立巢狀資料 Web 控制項，在另一個，特別檢查如何讓外部的重複項，顯示每個類別目錄項目與清單項目符號清單中的每個分類的產品內部的重複項。 建立巢狀的使用者介面的主要挑戰就在於存取和正確的資料繫結至內部資料 Web 控制項。 有各種技術，我們在本教學課程中所檢查的其中兩個。 檢查第一種方法中的外部資料 Web 控制項 s 使用 ObjectDataSource`ItemTemplate`的繫結至的內部資料 Web 控制項透過其`DataSourceID`屬性。 第二種技術存取資料，透過 ASP.NET 頁面 s 程式碼後置類別中的方法。 這個方法可以再繫結至內部資料 Web 控制項的`DataSource`透過資料繫結語法的屬性。

雖然您在本教學課程中檢查巢狀的使用者介面使用 Repeater 內變成巢狀的重複項，這些技術可以擴充至其他資料 Web 控制項。 您可以建立巢狀內一個 GridView 或 GridView 內的 DataList 和 Repeater 等等。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones 和 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [下一頁](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
