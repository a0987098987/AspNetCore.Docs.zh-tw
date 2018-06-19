---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: 使用兩個 DropDownLists (C#) 進行篩選的主要/詳細資料 |Microsoft 文件
author: rick-anderson
description: 本教學課程會展開主要/詳細資料關聯性加入第三個圖層，以選取所需的父系及祖系 recor 使用兩個 DropDownList 控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887248"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>主從式篩選搭配兩個 DropDownLists (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe)或[下載 PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> 本教學課程會展開，加入第三個圖層，選取所需的父系及祖系記錄使用兩個 DropDownList 控制項的主要/詳細資料關聯性。


## <a name="introduction"></a>簡介

在[上一個教學課程](master-detail-filtering-with-a-dropdownlist-cs.md)我們檢驗如何顯示簡單的主要/詳細資料報表，使用單一的 DropDownList 填入的類別，以及顯示屬於所選類別目錄的產品的 GridView。 此報表模式運作時顯示的記錄有一對多關聯性可以輕鬆地擴充到可用於包含多個一對多關聯性的案例。 例如，訂單輸入系統會有資料表對應到客戶、 訂單及訂單行項目。 指定的客戶可能會有多個訂單，與每筆訂單包含多個項目。 可以具有兩個 DropDownLists 和 GridView 使用者呈現這類資料。 第一個 DropDownList 必須清單項目為第二個資料庫中每位客戶的內容所選取的客戶所下的訂單。 GridView 會列出從選取的訂單明細項目。

Northwind 資料庫包含中的標準客戶/順序/訂單詳細資料資訊時其`Customers`， `Orders`，和`Order Details`資料表，這些資料表不我們架構中擷取。 然而，我們仍然可以說明使用兩個相依 DropDownLists。 第一個 DropDownList 會列出類別目錄以及第二個屬於所選類別目錄的產品。 在 DetailsView 會列出所選產品的詳細資料。

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>步驟 1： 建立和填入類別 DropDownList

我們第一個目標是要加入列出的類別目錄的 DropDownList。 這些步驟已仔細檢查先前的教學課程中，但此彙總為求完整起見。

開啟`MasterDetailsDetails.aspx`頁面`Filtering`資料夾中，將 DropDownList 加入至頁面上，設定其`ID`屬性`Categories`，然後按一下 設定資料來源中的連結其智慧標籤。 從資料來源組態精靈選擇加入新的資料來源。


[![加入新的資料來源的 DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**圖 1**： 加入新的資料來源的 DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


新的資料來源，當然應該 ObjectDataSource。 將這個新 ObjectDataSource `CategoriesDataSource` ，並讓它叫用`CategoriesBLL`物件的`GetCategories()`方法。


[![選擇使用 CategoriesBLL 類別](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**圖 2**： 選擇使用`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![設定為使用 GetCategories() 方法 ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**圖 3**： 設定要使用 ObjectDataSource`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


設定 ObjectDataSource 之後我們仍然需要指定哪一個資料來源欄位應該會顯示在`Categories`DropDownList 和哪一項應該設定為清單項目的值。 設定`CategoryName`欄位顯示器和`CategoryID`做為每個清單項目值。


[![此類別名稱欄位，然後使用 CategoryID 有 DropDownList 顯示做為值](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**圖 4**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


現在我們有 DropDownList 控制項 (`Categories`) 中的記錄會填入`Categories`資料表。 當使用者選擇新的類別從 DropDownList 我們會想要重新整理產品我們將在步驟 2 中建立的 DropDownList 發生回傳。 因此，勾選 啟用 AutoPostBack 選項，從`categories`DropDownList 的智慧標籤。


[![啟用 AutoPostBack 類別 DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**圖 5**： 啟用 AutoPostBack 如`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>步驟 2： 第二個 DropDownList 以顯示所選類別目錄的產品

與`Categories`DropDownList 完成下, 一步是要顯示屬於所選類別目錄的產品的 DropDownList。 若要完成這項作業，加入另一個 DropDownList 頁面名為`ProductsByCategory`。 如同`Categories`DropDownList，建立新的 ObjectDataSource `ProductsByCategory` DropDownList 名為`ProductsByCategoryDataSource`。


[![ProductsByCategory DropDownList 加入新的資料來源](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**圖 6**： 加入新的資料來源的`ProductsByCategory`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![建立名為 ProductsByCategoryDataSource 新 ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**圖 7**： 建立新的 ObjectDataSource 具名`ProductsByCategoryDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


因為`ProductsByCategory`DropDownList 以顯示屬於所選類別目錄，只要這些產品的需求已叫用 ObjectDataSource`GetProductsByCategoryID(categoryID)`方法從`ProductsBLL`物件。


[![選擇使用 ProductsBLL 類別](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**圖 8**： 選擇使用`ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![設定為使用 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**圖 9**： 設定要使用 ObjectDataSource`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


我們需要在精靈的最後一個步驟中指定的值*`categoryID`* 參數。 將此參數指派給選取的項目從`Categories`DropDownList。


[![從類別 DropDownList 提取 categoryID 參數值](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**圖 10**： 提取*`categoryID`* 參數值從`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


與設定 ObjectDataSource，剩下的只有指定哪些資料來源欄位可用來顯示和 DropDownList 項目的值。 顯示`ProductName`欄位，並使用`ProductID`欄位做為值。


[![指定用於 DropDownList ListItems 文字和值屬性的資料來源欄位](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**圖 11**： 指定資料來源欄位使用 DropDownList `ListItem` s'`Text`和`Value`屬性 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


與 ObjectDataSource 和`ProductsByCategory`DropDownList 設定我們的頁面會顯示兩個 DropDownLists： 第二個會列出這些屬於所選類別目錄的產品時，第一個會列出所有的類別。 當使用者從第一個 DropDownList 選取新的類別時，將發生回傳，第二個 DropDownList 會重新繫結，顯示屬於新選取的分類的產品。 圖 12 和 13 顯示`MasterDetailsDetails.aspx`中透過瀏覽器檢視時的動作。


[![當第一次造訪的頁面時，所選取的飲料類別](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**圖 12**： 當第一次造訪的頁面時，所選取的飲料類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![選擇不同的類別目錄會顯示新的分類產品](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**圖 13**： 選擇不同的類別顯示新的分類產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


目前`productsByCategory`DropDownList 變更時，沒有*不*導致回傳。 不過，我們會需要發生在我們新增以顯示所選的產品的詳細資料 (步驟 3) 的 DetailsView 回傳。 因此，選取 啟用 AutoPostBack 核取方塊，從`productsByCategory`DropDownList 的智慧標籤。


[![啟用 productsByCategory DropDownList AutoPostBack 功能](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**圖 14**： 啟用 AutoPostBack 功能，以便`productsByCategory`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>步驟 3： 使用 DetailsView 以顯示所選產品的詳細資料

最後一個步驟是在 DetailsView 中顯示所選產品的詳細資料。 若要達成此目的，將在 DetailsView 加入頁面中，設定其`ID`屬性`ProductDetails`，並為其建立新的 ObjectDataSource。 設定此 ObjectDataSource 提取資料的來源`ProductsBLL`類別的`GetProductByProductID(productID)`方法使用的選取的值`ProductsByCategory`值的 DropDownList *`productID`* 參數。


[![選擇使用 ProductsBLL 類別](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**圖 15**： 選擇使用`ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![設定為使用 GetProductByProductID(productID) 方法 ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**圖 16**： 設定要使用 ObjectDataSource`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![從 ProductsByCategory DropDownList 提取 productID 參數值](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**圖 17**： 提取*`productID`* 參數值從`ProductsByCategory`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


您可以選擇在 DetailsView 中顯示任何可用的欄位。 我已選擇移除`ProductID`， `SupplierID`，和`CategoryID`欄位重新排列及格式化剩下的欄位。 此外，out DetailsView 的清除`Height`和`Width`屬性，可讓 DetailsView 展開至最佳顯示效果所需的寬度，其資料，而不是需要它限制為指定的大小。 完整的標記顯示如下：


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

請花一點時間試用`MasterDetailsDetails.aspx`瀏覽器中的。 第一眼看起來它可能會出現，一切運作所需，但沒有微妙的問題。 當您選擇新的分類`ProductsByCategory`DropDownList 會更新來包含這些產品所選取的類別目錄，但`ProductDetails`DetailsView 持續顯示前一個產品資訊。 當您選擇不同的產品所選取類別目錄時，會更新 DetailsView。 此外，如果夠徹底測試，您會看到，如果您持續選擇新的類別目錄 (例如選擇飲料`Categories`DropDownList，則 「 調味品 」，然後 Confections) 每個類別選項會導致`ProductDetails`DetailsView 重新整理。

若要協助具體化這個問題，讓我們看看特定範例。 當您第一次瀏覽網頁時所選取的飲料類別和相關的產品中載入`ProductsByCategory`DropDownList。 Chai 是選取的產品，而且其詳細資料會顯示在`ProductDetails`DetailsView，如圖 18 所示。


[![在 DetailsView 中會顯示選取的產品詳細資料](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**圖 18**: 選取產品的詳細資料會顯示在 DetailsView ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


如果您將從飲料的類別選項變更為 「 調味品 」 時，就會發生回傳而`ProductsByCategory`DropDownList 更新同理，但有 DetailsView Chai 為仍然顯示詳細資料。


[![仍然顯示的先前選取產品的詳細資料](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**圖 19**： 仍然顯示的先前選取產品的詳細資料 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


挑選新的產品，從清單重新整理 DetailsView 如預期般。 如果您變更產品之後挑選新的類別，在 DetailsView 一次將不會重新整理。 不過，如果而不是選擇新的產品，您會選取新的類別，就會重新整理 DetailsView。 在世界中的項目呢？

問題是網頁的生命週期中的時間問題。 每次要求頁面時透過其轉譯為幾個步驟進行。 在其中一個步驟 ObjectDataSource 控制檢查，看看是否有任何其`SelectParameters`值已變更。 因此，此資料 Web 控制項繫結至 ObjectDataSource 知道它必須重新整理其顯示。 例如，當新的類別目錄選取時， `ProductsByCategoryDataSource` ObjectDataSource 偵測到它的參數值已變更而`ProductsByCategory`DropDownList 重新繫結本身，取得所選取類別目錄的產品。

在此情況下，就會發生此問題是 ObjectDataSources 檢查是否有變更參數的頁面生命週期中的點就會發生*之前*的相關資料的 Web 控制項重新繫結。 因此，當您選取新的類別時`ProductsByCategoryDataSource`ObjectDataSource 偵測到它的參數值中的變更。 使用 Sqldatasource `ProductDetails` DetailsView，不過，不會注意任何這類變更因為`ProductsByCategory`DropDownList 尚未重新繫結。 稍後的生命週期`ProductsByCategory`DropDownList 重新繫結至其抓取新選取的分類的產品的 Sqldatasource。 雖然`ProductsByCategory`DropDownList 的值已變更， `ProductDetails` DetailsView 的 ObjectDataSource 完成其參數值檢查; 因此，在 DetailsView 會顯示先前的結果。 圖 20 會說明這種互動。


[![ProductsByCategory DropDownList 值變更之後 ProductDetails DetailsView ObjectDataSource 檢查變更](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**圖 20**: `ProductsByCategory` DropDownList 值變更之後`ProductDetails`DetailsView 的 ObjectDataSource 檢查變更 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


若要補救這個我們需要明確地重新繫結`ProductDetails`之後 DetailsView `ProductsByCategory` DropDownList 已經繫結。 我們可以完成此作業藉由呼叫`ProductDetails`DetailsView 的`DataBind()`方法時`ProductsByCategory`DropDownList 的`DataBound`事件引發。 將下列事件處理常式程式碼加入`MasterDetailsDetails.aspx`網頁的程式碼後置類別 (請參閱"[以程式設計方式設定 ObjectDataSource 參數值](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"如需有關如何加入事件處理常式的討論):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

此明確呼叫之後`ProductDetails`DetailsView 的`DataBind()`方法已經加入，本教學課程可以正常運作。 圖 21 會反白顯示已變更如何補救我們先前的問題。


[![ProductDetails DetailsView 會明確地重新整理時 ProductsByCategory DropDownList 的資料繫結事件引發](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**圖 21**: `ProductDetails` DetailsView 時，明確地重新整理`ProductsByCategory`DropDownList 的`DataBound`事件引發 ([按一下以檢視完整大小的影像](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>總結

DropDownList 做為主要/詳細資料報表的理想的使用者介面項目之間的主要和詳細的記錄是一對多關聯性。 在前述教學課程中，我們看到如何使用單一的 DropDownList 篩選所選取的類別顯示的產品。 本教學課程中我們產品的 GridView 取代 DropDownList，並用 DetailsView 來顯示所選產品的詳細資料。 在本教學課程所討論的概念可以輕鬆地擴充到資料模型包含多個一對多關聯性，例如： 客戶、 訂單及訂單項目。 一般情況下，您可以隨時加入 DropDownList 每一對多關聯性中的 「 一 」 實體中。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-filtering-with-a-dropdownlist-cs.md)
> [下一頁](master-detail-filtering-across-two-pages-cs.md)
