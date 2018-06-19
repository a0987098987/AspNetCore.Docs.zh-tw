---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: 巢狀的資料 Web 控制項 (C#) |Microsoft 文件
author: rick-anderson
description: 在此教學課程中我們將探討如何使用中繼器在另一個中繼器巢狀。 這些範例將說明如何填入內部中繼器這兩個 d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 4957f555691efaeaafa5bcf92141e0bef1cb1de9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875847"
---
<a name="nested-data-web-controls-c"></a>巢狀的資料的 Web 控制項 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe)或[下載 PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> 在此教學課程中我們將探討如何使用中繼器在另一個中繼器巢狀。 這些範例將說明如何以宣告方式和以程式設計方式填入內部的重複項。


## <a name="introduction"></a>簡介

除了靜態 HTML 和資料繫結語法，範本也可以包含 Web 控制項和使用者控制項。 這些 Web 控制項可以有其屬性指派透過宣告式、 資料繫結語法，或在適當的伺服器端事件處理常式中可以透過程式設計方式存取。

內嵌在樣板內控制項，可以自訂並改善了的外觀和使用者體驗。 例如，在[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程，我們可了解如何藉由新增日曆控制項中顯示員工的雇用日期為 TemplateField; 中自訂 GridView 的顯示[加入驗證控制項的編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)和[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程中，我們可了解如何自訂編輯和插入介面加入驗證控制項、 文字方塊、 DropDownLists 和其他 Web 控制項。

範本也可以包含其他 Web 控制項的資料。 也就是說，我們可以包含其他 DataList （或中繼器或 GridView 或 DetailsView，等等） 內其範本 DataList。 這類介面的挑戰將適當的資料繫結至 Web 控制項的內部資料。 另外還有幾個不同的方法，從使用程式設計項目 ObjectDataSource 的宣告式選項。

在此教學課程中我們將探討如何使用中繼器在另一個中繼器巢狀。 外部中繼器會包含在資料庫中，每個類別目錄項目顯示的類別 s 的名稱和描述。 每個類別目錄項目 s 內部中繼器會顯示屬於該類別每項產品的資訊 （請參閱圖 1） 中的項目清單。 我們的範例將說明如何以宣告方式和以程式設計方式填入內部的重複項。


[![列出每個類別目錄，以及其產品中，](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**圖 1**： 每個類別，以及其產品中，會列出 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>步驟 1： 建立類別目錄清單

當建置會使用頁面巢狀的 Web 控制項的資料時，我發現很有幫助設計、 建立及測試最外層資料 Web 控制項第一次，而不需甚至擔心內部的巢狀控制項。 因此，可讓 s 帶您逐步完成新增中繼器的名稱和描述每個類別目錄會列出的頁面所需的步驟開始。

先開啟`NestedControls.aspx`頁面`DataListRepeaterBasics`資料夾並將中繼器控制項加入至頁面上，設定其`ID`屬性`CategoryList`。 中繼器 s 智慧標籤，選擇以建立名為新 ObjectDataSource `CategoriesDataSource`。


[![命名新 ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**圖 2**： 命名新 ObjectDataSource `CategoriesDataSource` ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image6.png))


設定 ObjectDataSource 提取資料的來源`CategoriesBLL`類別的`GetCategories`方法。


[![設定為使用 CategoriesBLL 類別的 GetCategories 方法 ObjectDataSource](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories`方法 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image9.png))


若要指定中繼器的範本內容我們需要移至來源檢視，並以手動方式輸入的宣告式語法。 新增`ItemTemplate`顯示中的類別目錄的名稱`<h4>`項目和類別目錄的描述段落項目中 (`<p>`)。 此外，o d e s 會分隔每個類別目錄與水平規則 (`<hr>`)。 進行這些變更後您的頁面應包含宣告式語法中繼器和 ObjectDataSource 與下列類似：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

圖 4 顯示我們透過瀏覽器檢視時的進度。


[![每個類別名稱和描述列出，以水平尺規分隔](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**圖 4**： 每個類別目錄名稱和描述列出，以水平尺規分隔 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>步驟 2： 加入巢狀的產品中繼器

列出完整的類別目錄，與下一步是加入至中繼器`CategoryList`s`ItemTemplate`會顯示屬於適當的分類這些產品的相關資訊。 有數種方式，我們可以針對此內部的重複項，其中兩個我們將探討短時間內擷取資料。 現在，讓 s 只建立產品中繼器內`CategoryList`中繼器的`ItemTemplate`。 具體來說，可讓 s 具備中繼器顯示項目符號清單中的每項產品，與每個清單項目包括 s 產品名稱和價格的產品。

若要建立我們需要手動輸入內部中繼器 s 宣告式語法和範本到此中繼器`CategoryList`s `ItemTemplate`。 加入下列標記內`CategoryList`中繼器的`ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>步驟 3： 繫結至 ProductsByCategoryList 中繼器的特定類別的產品

如果您在此時造訪透過瀏覽器頁面，您的畫面看起來相同如圖 4 所示因為我們尚未將任何資料繫結至中繼器的發生。 有幾種方式，我們可以用來擷取適當的產品記錄，並將其繫結至中繼器，比其他一些更有效率。 此處的主要挑戰變回適當的產品為指定的類別目錄。

存取的資料繫結至內部中繼器控制項可以是以宣告方式，透過在 ObjectDataSource`CategoryList`中繼器的`ItemTemplate`，或以程式設計的方式，從 ASP.NET 頁面 s 程式碼後置頁面。 同樣地，這項資料可繫結內部中繼器可能是以宣告方式-透過內部中繼器 s`DataSourceID`屬性或透過宣告式資料繫結語法或以程式設計方式參考內部中繼器中的`CategoryList`中繼器 s`ItemDataBound`事件處理常式，以程式設計方式設定其`DataSource`屬性，並呼叫其`DataBind()`方法。 可讓 s 瀏覽任一種方式。

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>存取以宣告方式使用 ObjectDataSource 控制項的資料和`ItemDataBound`事件處理常式

因為我們已使用廣泛地在此教學課程系列最合理的選擇，以存取資料，如這個範例是堅持使用 ObjectDataSource ObjectDataSource。 `ProductsBLL`類別具有`GetProductsByCategoryID(categoryID)`方法會傳回屬於指定這些產品的相關資訊*`categoryID`*。 因此，我們可以加入至 ObjectDataSource`CategoryList`中繼器的`ItemTemplate`並將它設定為從這個類別的方法存取它的資料。

不幸的是，中繼器不允許其範本，因此我們必須以手動方式加入此 ObjectDataSource 控制項的宣告式語法，透過 [設計] 檢視進行編輯。 下列語法顯示`CategoryList`中繼器 s`ItemTemplate`加入此新 ObjectDataSource 之後 (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

我們使用 ObjectDataSource 方法時要設定`ProductsByCategoryList`中繼器 s`DataSourceID`屬性`ID`的 ObjectDataSource (`ProductsByCategoryDataSource`)。 此外，請注意，我們 ObjectDataSource 具有`<asp:Parameter>`項目，指定*`categoryID`* 值，會傳遞至`GetProductsByCategoryID(categoryID)`方法。 但我們該如何指定這個值嗎？ 在理想情況下，我們 d 可以只設定`DefaultValue`屬性`<asp:Parameter>`使用資料繫結語法項目如下所示：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

不幸的是，這只適用於控制項具有資料繫結語法`DataBinding`事件。 `Parameter`類別缺少這類事件，而因此上述語法是不合法的而且會導致執行階段錯誤。

若要設定此值，我們需要建立事件處理常式`CategoryList`中繼器的`ItemDataBound`事件。 請記得，`ItemDataBound`事件引發一次針對每個繫結至中繼器的項目。 因此，針對外部中繼器就會引發此事件每次我們可以指派目前`CategoryID`值設定為`ProductsByCategoryDataSource`ObjectDataSource 的`CategoryID`參數。

建立事件處理常式`CategoryList`中繼器的`ItemDataBound`為下列程式碼的事件：


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

這個事件處理常式一開始會確保我們重新處理資料項目而不是頁首、 頁尾或分隔符號的項目。 接下來，我們參考實際`CategoriesRow`剛才已繫結至目前的執行個體`RepeaterItem`。 最後，我們會參考在 ObjectDataSource`ItemTemplate`並指派其`CategoryID`參數值以`CategoryID`的目前`RepeaterItem`。

與此事件處理常式，`ProductsByCategoryList`中繼器中每個`RepeaterItem`繫結至這些產品的`RepeaterItem`的類別。 圖 5 顯示產生的輸出的螢幕擷取畫面。


[![外部中繼器列出每個類別目錄。內部的其中一個該類別目錄列出的產品](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**圖 5**: 外部中繼器列出每個類別目錄; 內部一個列出該分類的產品 ([按一下以檢視完整大小的影像](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>以程式設計方式存取的產品類別目錄資料

而不是使用來擷取目前的分類的產品的 Sqldatasource，我們可以在 ASP.NET 頁面 s 程式碼後置類別中建立方法 (或在`App_Code`資料夾或個別的類別庫專案中) 會傳回適當的集合產品時傳入`CategoryID`。 假設我們具有這種方法，我們 ASP.NET 頁面 s 程式碼後置類別中，並且名為`GetProductsInCategory(categoryID)`。 使用此方法就地我們可以使用下列的宣告式語法內部中繼器繫結目前分類的產品：


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

中繼器 s`DataSource`屬性以表示其資料來自於使用資料繫結語法`GetProductsInCategory(categoryID)`方法。 因為`Eval("CategoryID")`傳回值的型別`Object`，我們將物件轉換成`Integer`傳遞到前`GetProductsInCategory(categoryID)`方法。 請注意，`CategoryID`存取透過資料繫結語法如下`CategoryID`中*外部*中繼器 (`CategoryList`)，一個 s 繫結中的記錄至`Categories`資料表。 因此，我們知道`CategoryID`不是資料庫`NULL`值，這就是為什麼我們可以盲目地轉型`Eval`方法，而不檢查是否我們重新處理`DBNull`。

使用此方法時，我們需要建立`GetProductsInCategory(categoryID)`方法並擷取適當的產品提供提供一組*`categoryID`*。 可以執行此作業，只傳回`ProductsDataTable`傳回`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 可讓建立`GetProductsInCategory(categoryID)`方法的程式碼後置類別中我們`NestedControls.aspx`頁面。 請使用下列程式碼：


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

這個方法只會建立的執行個體`ProductsBLL`方法傳回的結果和`GetProductsByCategoryID(categoryID)`方法。 請注意方法必須標記為`Public`或`Protected`; 如果方法已標記為`Private`，將無法再存取從 ASP.NET 頁面 s 宣告式標記。

若要使用這個新的技術，請花點時間檢視透過瀏覽器頁面變更後。 使用 ObjectDataSource 時的輸出應該是相同的輸出和`ItemDataBound`事件處理常式方法 （請參閱上一步圖 5，請參閱螢幕擷取畫面）。

> [!NOTE]
> 它看起來好像建立 busywork `GetProductsInCategory(categoryID)` ASP.NET 頁面 s 程式碼後置類別中的方法。 在所有情況下，這個方法只會建立的執行個體`ProductsBLL`類別，並傳回結果的其`GetProductsByCategoryID(categoryID)`方法。 為何不只需要呼叫這個方法直接從資料繫結中的語法內部中繼器，像是： `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`？ 雖然這種語法贏了 t 我們目前的實作使用`ProductsBLL`類別 (因為`GetProductsByCategoryID(categoryID)`方法是執行個體方法)，您可以修改`ProductsBLL`包含靜態`GetProductsByCategoryID(categoryID)`方法或類別加入靜態`Instance()`方法傳回的新執行個體`ProductsBLL`類別。


雖然這類的修改，就不需要`GetProductsInCategory(categoryID)`ASP.NET 頁面 s 程式碼後置類別中的方法，在程式碼後置類別的方法為我們提供更靈活地使用資料擷取，我們很快就會看到。

## <a name="retrieving-all-of-the-product-information-at-once"></a>擷取的所有產品資訊一次

兩個舊技術我們 ve 檢查抓取這些產品的目前類別藉由呼叫`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 (第一種方法之前透過 ObjectDataSource，透過第二個`GetProductsInCategory(categoryID)`中的方法程式碼後置類別）。 這個方法會叫用，每次呼叫資料存取層中，向 「 商務邏輯層的查詢傳回的資料列的 SQL 陳述式的資料庫`Products`資料表其`CategoryID`欄位符合提供的輸入的參數。

指定*N*系統中的類別，這種方法通道*N* + 1 呼叫資料庫有一個資料庫查詢，以取得所有的類別，然後*N*呼叫，以取得產品每個類別目錄所特有。 我們可以不過，擷取所有所需的資料中只有兩個資料庫來取得所有類別，以取得所有產品的另一種呼叫一次呼叫。 一旦所有產品，我們可以因此都篩選這些產品只比對目前的產品`CategoryID`繫結至該類別 s 內部的重複項。

若要提供這項功能，我們只需要進行稍微修改一下`GetProductsInCategory(categoryID)`我們 ASP.NET 頁面 s 程式碼後置類別中的方法。 而不是盲目地傳回的結果`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法中，我們可以改為先存取*所有*產品的 (如果它們不 t 已已經存取)，然後傳回只在已篩選的檢視在傳入的基礎產品`CategoryID`。


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

請注意頁面層級變數的加法`allProducts`。 這個保留的所有產品的相關資訊，並在第一次擴展`GetProductsInCategory(categoryID)`叫用方法。 在確定`allProducts`物件已建立並填入，方法會使其只包括資料列篩選 DataTable 的結果`CategoryID`符合指定`CategoryID`存取。 這種方法可減少從存取資料庫的次數*N* + 1 到兩個。

這項增強功能不會產生任何變更要轉譯的頁面上，標記，也不會將另一種方法比傳回較少的記錄。 而是只會呼叫資料庫數目。

> [!NOTE]
> 其中一個直覺的方式可能原因減少資料庫的存取會主力語言會提升效能。 不過，這可能不是大小寫。 如果您有大量產品其`CategoryID`是`NULL`，如範例中，然後呼叫`GetProducts`方法會傳回永遠不會顯示的產品數目。 此外，傳回所有產品可以都是浪費如果您重新只顯示一個類別目錄，都可能的情況下，如果您已實作分頁子集。


一律，當談到分析兩種技術的效能，因為只有笑話量值就是執行受到控制的測試您的應用程式 s 常見案例為量身訂做。

## <a name="summary"></a>總結

在此教學課程中我們了解如何將巢狀處理一個資料在另一個，Web 控制項特別檢查如何以顯示每個類別目錄項目與清單項目符號清單中的每個分類的產品內部中繼器外部中繼器。 建立巢狀的使用者介面的主要挑戰就在於存取和正確的資料繫結至 Web 控制項內部的資料。 有各種不同的技術，我們在本教學課程所檢查的兩個。 檢查第一種方法則用於外部資料 Web 控制項 s ObjectDataSource`ItemTemplate`的繫結到內部資料的 Web 控制項，透過其`DataSourceID`屬性。 第二項技術存取資料，透過 ASP.NET 頁面 s 程式碼後置類別中的方法。 這個方法可以再繫結至內部資料 Web 控制項的`DataSource`透過資料繫結語法的屬性。

雖然檢查本教學課程中的巢狀的使用者介面使用巢狀方式置於中繼器中繼器中，這些技術可以擴充至其他資料 Web 控制項。 您可以建立巢狀 GridView 或在 DataList 內的 GridView 內中繼器等等。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones 和 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [下一頁](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
