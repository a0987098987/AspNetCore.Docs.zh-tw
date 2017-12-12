---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: "在 DataList 或中繼器控制項中 (C#) 中的報表資料分頁 |Microsoft 文件"
author: rick-anderson
description: "DataList 都中繼器的供應項目自動分頁或時排序支援，本教學課程會示範如何將分頁支援新增至 DataList 或中繼器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 783557b69486c284a6ed927e32e71cb602695080
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>DataList 或中繼器控制項中 (C#) 中的分頁報表資料
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe)或[下載 PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> 雖然 DataList 都中繼器優惠自動分頁，或排序支援，本教學課程會示範如何加入 DataList 或中繼器，允許更有彈性的分頁及資料顯示介面中的分頁支援。


## <a name="introduction"></a>簡介

分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。 比方說，搜尋時在線上書店 ASP.NET 書籍，可能有數百個這類的書籍，但列出的搜尋結果此報表會列出每個頁面僅十個相符項目。 此外，可以根據標題、 價格、 頁面計數、 作者姓名等排序結果。 如我們所述[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教學課程中，所有提供內建的刻度核取方塊可啟用的分頁支援的 GridView、 DetailsView 和在 FormView 控制項。 在 GridView 也包含排序支援。

不幸的是，DataList 和中繼器都不會提供自動分頁或排序支援。 在本教學課程中，我們將檢驗如何加入 DataList 或中繼器的分頁支援。 我們必須手動建立分頁介面，顯示的記錄，以適當的頁面，並請記得在回傳所造訪的頁面。 雖然這採用更多的時間和比與 GridView、 DetailsView 或在 FormView 的程式碼，DataList 和中繼器允許更有彈性的分頁及資料顯示介面。

> [!NOTE]
> 此教學課程著重於分頁。 在下一個教學課程中我們將會開啟我們注意到加入排序功能。


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步驟 1： 加入分頁和排序教學課程的 Web 網頁

開始本教學課程之前，可讓 s 先花點時間加入 ASP.NET 網頁，我們需要針對本教學課程和下一個步驟。 藉由建立新的資料夾中名為專案啟動`PagingSortingDataListRepeater`。 接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![建立 PagingSortingDataListRepeater 資料夾，以及新增的教學課程的 ASP.NET 頁面](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**圖 1**： 建立`PagingSortingDataListRepeater`資料夾並加入的教學課程的 ASP.NET 網頁


接下來，開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。 此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並在項目符號清單中目前區段中顯示這些教學課程。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


若要顯示分頁和排序教學課程，我們將建立的項目符號清單後，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，然後加入下列標記的編輯和刪除之後，DataList 站台對應節點標記：


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**圖 3**： 更新包含新的 ASP.NET 網頁站台對應


## <a name="a-review-of-paging"></a>將分頁的檢閱

在先前的教學課程中我們可了解如何透過 GridView、 DetailsView 和 FormView 控制項中的資料頁面上。 這三個控制項提供簡單的分頁稱為*預設分頁*，可以實作只檢查控制項 s 智慧標籤中的啟用分頁選項。 使用預設分頁時，每次要求的資料頁面時的第一頁上瀏覽或當使用者巡覽至其他資料 GridView 中，在 DetailsView 的頁面或 FormView 控制項重新要求*所有*的資料ObjectDataSource。 它然後剪取時来顯示的記錄的特定集給定要求的頁面索引和每頁顯示的記錄數目。 預設分頁中詳細討論[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教學課程。

預設分頁重新要求每個頁面的所有記錄，因為它並不實用時不夠大量的資料進行分頁。 例如，想像一下頁面大小為 10 的 50,000 筆記錄進行分頁。 每次使用者移到新的頁面上，必須擷取從資料庫中所有的 50,000 筆記錄，即使其中只有 10 會顯示。

*自訂分頁*透過抓取精確子集所要求的頁面上顯示的記錄，以解決效能問題的預設分頁。 在實作自訂分頁時，我們必須撰寫 SQL 查詢，有效率地傳回只在正確的記錄集。 我們了解如何建立這類查詢使用新的 SQL Server 2005 s [ `ROW_NUMBER()`關鍵字](http://www.4guysfromrolla.com/webtech/010406-1.shtml)回[有效率地分頁透過大型量的資料](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教學課程。

若要在 DataList 或中繼器控制項中實作預設分頁，我們可以使用[`PagedDataSource`類別](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.aspx)周圍的包裝函式`ProductsDataTable`其內容正在呼叫。 `PagedDataSource`類別具有`DataSource`可以指派給任何可列舉物件的屬性和[ `PageSize` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx)和[ `CurrentPageIndex` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx)屬性來指示要記錄數顯示每個頁面和目前頁面索引。 一旦已設定這些屬性，`PagedDataSource`可用來當做資料來源的 Web 控制項的任何資料。 `PagedDataSource`、 列舉時，就只能傳回適當的記錄其內部子集`DataSource`根據`PageSize`和`CurrentPageIndex`屬性。 圖 4 說明的功能`PagedDataSource`類別。


![PagedDataSource 包裝的可分頁介面的可列舉物件](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**圖 4**:`PagedDataSource`包裝的可分頁介面的可列舉物件


`PagedDataSource`物件可以建立和設定直接從商務邏輯層和繫結至 DataList 或中繼器透過 ObjectDataSource，或可以建立並設定直接在 ASP.NET 網頁 s 程式碼後置類別中。 如果第二種方法，我們必須放棄使用 ObjectDataSource，改為將資料繫結分頁 DataList 或中繼器以程式設計的方式。

`PagedDataSource`物件也有屬性，以支援自訂分頁。 不過，我們就可以略過使用`PagedDataSource`用於自訂分頁，因為我們已經有 BLL 方法`ProductsBLL`針對傳回的精確記錄，若要顯示的自訂分頁所設計的類別。

在此教學課程中我們將探討將新方法加入 DataList 中實作預設分頁`ProductsBLL`傳回適當設定的類別`PagedDataSource`物件。 在下一個教學課程中，我們會看到如何使用自訂分頁。

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>步驟 2： 加入商務邏輯層中的預設分頁方法

`ProductsBLL`類別目前已傳回所有的產品資訊的方法`GetProducts()`，另一個用於傳回產品起始的索引處的特定子集`GetProductsPaged(startRowIndex, maximumRows)`。 使用預設分頁 GridView、 DetailsView 和 FormView 控制所有使用`GetProducts()`方法來擷取所有的產品，但接著使用`PagedDataSource`在內部以顯示記錄的正確子集合。 若要複寫的 DataList 和中繼器控制項與這項功能，我們可以建立新的方法中模擬此行為 BLL。

將方法加入`ProductsBLL`名為類別`GetProductsAsPagedDataSource`會採用兩個整數的輸入參數：

- `pageIndex`若要顯示，頁面的索引編製索引零，並
- `pageSize`每頁顯示的記錄數目。

`GetProductsAsPagedDataSource`一開始會擷取*所有*記錄的`GetProducts()`。 然後它會建立`PagedDataSource`物件，並設定其`CurrentPageIndex`和`PageSize`屬性之值的傳入`pageIndex`和`pageSize`參數。 此方法結束時，會傳回這設定`PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>步驟 3： 中使用的預設分頁 DataList 顯示產品的資訊

與`GetProductsAsPagedDataSource`方法加入至`ProductsBLL`類別中，我們現在可以建立 DataList 或中繼器提供預設分頁。 先開啟`Paging.aspx`頁面`PagingSortingDataListRepeater`資料夾，然後從 [工具箱] 拖曳至設計工具，設定 DataList 的拖曳 DataList`ID`屬性`ProductsDefaultPaging`。 從 DataList s 智慧標籤，建立名為新 ObjectDataSource`ProductsDefaultPagingDataSource`並加以設定，讓它會擷取使用資料`GetProductsAsPagedDataSource`方法。


[![建立 ObjectDataSource，並將它設定為使用 GetProductsAsPagedDataSource （） 方法](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**圖 5**： 建立 ObjectDataSource，並將它設定為使用`GetProductsAsPagedDataSource``()`方法 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![設定下拉式清單中更新、 插入和刪除索引標籤為 （無）](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**圖 6**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


因為`GetProductsAsPagedDataSource`方法必須要有兩個輸入的參數，則精靈會提示我們為這些參數值的來源。

頁面索引和頁面大小值必須記住在回傳。 它們可以儲存在檢視狀態、 保存至查詢字串、 儲存在工作階段變數或記住使用其他方法。 此教學課程中我們將使用的 querystring，允許特定的頁面上的資料會設為書籤的優點。

特別是，使用的 querystring 欄位 pageIndex 和 pageSize 的`pageIndex`和`pageSize`參數，分別 （請參閱圖 7）。 為贏了 t 的查詢字串值時必須存在使用者第一次造訪此頁，請花一點時間來設定這些參數，預設值。 如`pageIndex`，設定的預設值為 0 （這會顯示資料的第一頁） 和`pageSize`的預設值為 4。


[![PageIndex 和 pageSize 參數作為來源的查詢字串](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**圖 7**： 做為來源使用查詢字串`pageIndex`和`pageSize`參數 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


設定後 ObjectDataSource，Visual Studio 會自動建立`ItemTemplate`的資料清單。 自訂`ItemTemplate`以便顯示只有 s 產品名稱、 類別和供應商。 也設定 DataList s`RepeatColumns`屬性為 2，其`Width`為 100%，且其`ItemStyle`s`Width`為 50%。 這些寬度設定會提供兩個資料行的相等間距。

進行這些變更之後，DataList 和 ObjectDataSource 的標記看起來應該如下所示：


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> 因為我們不會執行任何更新或刪除在此教學課程中的功能，您可能會停用 DataList 的檢視狀態，以減少轉譯的頁面大小。


一開始都造訪瀏覽器，透過此頁面時`pageIndex`也`pageSize`querystring 參數所提供。 因此，會使用預設值 0 到 4。 如圖 8 所示，這會導致顯示的前四個產品資料清單。


[![列出第四個產品](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**圖 8**： 所列的第四個產品 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


沒有分頁介面，那里目前無法直接 s 表示使用者可以瀏覽至第二個資料頁。 我們將在步驟 4 中建立分頁介面。 現在，不過，分頁僅可藉由直接於 querystring 中指定的分頁準則。 例如，若要檢視的第二頁，將變更 s 瀏覽器位址列中的 URL`Paging.aspx`至`Paging.aspx?pageIndex=2`並按下 Enter。 這會導致要顯示資料的第二頁 （請參閱圖 9）。


[![第二個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**圖 9**: 第二個頁面的資料會顯示 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>步驟 4： 建立分頁介面

有各種不同的分頁介面可以實作。 GridView、 DetailsView 和 FormView 控制項提供四個不同的介面之間進行選擇：

- **接下來，先前**使用者可以一次到一個或下一個一個移動一頁。
- **下一步 上, 一步中，最後一個**除了下一步 和 上一步 按鈕，這個介面包含移動至第一個或最後一個頁面的第一個和最後一個按鈕。
- **數值**列出頁面中的數字的分頁介面，讓使用者快速跳至特定頁面。
- **數值，第一次，最後一個**數值的頁碼，除了包含 移至第一個或最後一個頁面上的按鈕。

在 DataList 和中繼器中，我們會負責決定分頁介面和實作它。 這牽涉到頁面以建立所需的 Web 控制項，然後按一下特定的分頁介面按鈕時顯示要求的頁面。 此外，某些分頁介面控制項可能需要停用。 比方說，檢視時使用下一個資料的第一頁上, 一步，第一次，最後一個介面，第一個和上一步 按鈕會變成停用。

本教學課程，可讓的會使用下一步 上一步，第一次，最後的介面。 將四個按鈕 Web 控制項加入至頁面，並設定其`ID`s `FirstPage`， `PrevPage`， `NextPage`，和`LastPage`。 設定`Text`屬性&lt;&lt;第一次， &lt; Prev、 下一步&gt;，和最後一個&gt; &gt; 。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

接下來，建立`Click`鎯瓾餂鈕每個事件處理常式。 稍後我們會將新增顯示要求的網頁所需的程式碼。

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>記住透過正在呼叫的記錄總數

選取分頁介面，不論我們需要計算，並請記得透過正在呼叫的記錄總數。 總計資料列計數 （以頁面大小搭配） 會決定總頁數的資料正在呼叫，以決定哪些分頁介面控制項加入或已啟用。 在下一步 上, 一步，第一個，最後一個介面，我們在建立時，頁面計數使用兩種方式：

- 若要判斷是否我們要在此情況下檢視最後一頁中，會停用下一步] 和 [最後一個按鈕。
- 如果使用者按一下最後一個按鈕我們要的最後一個 whisk 它們小於頁面的頁面上，其索引是其中一個計數。

總計資料列計數上限除以頁面大小被計算頁面計數。 例如，如果我們會透過與每個分頁的四個記錄可包含 79 記錄的分頁，則頁面計數是 20 (79 上限 / 4)。 如果我們使用數字的分頁介面，這項資訊會通知我們對於多少數值頁面按鈕以顯示;如果我們分頁的介面包含下一個或最後一個 按鈕，頁面計數用來判斷何時停用的下一個或最後一個按鈕。

如果分頁介面將包含最後一個按鈕，請務必透過正在呼叫的記錄總數會記住在回傳，讓最後一個按鈕按下時，我們就可以判斷的最後一個頁面索引。 若要達成此目的，建立`TotalRowCount`保存其檢視狀態的值，ASP.NET 頁面 s 程式碼後置類別中的屬性：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

除了`TotalRowCount`、 花點時間建立唯讀頁面層級的屬性，輕鬆地存取的頁面索引、 頁面大小和頁面計數：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>決定要透過分頁的記錄總數

`PagedDataSource`物件從 ObjectDataSource s 傳回`Select()`方法已在其中*所有*一個產品記錄，即使它們的子集會顯示在資料清單。 `PagedDataSource` s [ `Count`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.count.aspx)傳回只會顯示在 DataList; 的項目數目[`DataSourceCount`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx)傳回內的項目總數`PagedDataSource`. 因此，我們需要指派 ASP.NET 頁面 s`TotalRowCount`屬性值的`PagedDataSource`s`DataSourceCount`屬性。

若要達成此目的，建立事件處理常式 ObjectDataSource s`Selected`事件。 在`Selected`都可存取 ObjectDataSource s 的傳回值的事件處理常式`Select()`方法在此情況下， `PagedDataSource`。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>顯示的資料要求的頁面

當使用者按一下其中一個按鈕分頁介面中時，我們要顯示的資料要求的頁面。 因為分頁參數透過查詢字串，指定要顯示的資料使用要求的頁面`Response.Redirect(url)`有使用者 s 瀏覽器重新要求`Paging.aspx`以適當的分頁參數頁面。 例如，若要顯示資料的第二個頁面，我們會重新導向使用者`Paging.aspx?pageIndex=1`。

若要達成此目的，建立`RedirectUser(sendUserToPageIndex)`方法，將使用者重新導向`Paging.aspx?pageIndex=sendUserToPageIndex`。 然後，呼叫這個方法從四個按鈕`Click`事件處理常式。 在`FirstPage``Click`事件處理常式，此時會呼叫`RedirectUser(0)`，以將它們傳送至第一頁，則在`PrevPage``Click`事件處理常式，使用`PageIndex - 1`做為頁面的索引;，依此類推。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

與`Click`事件處理常式完成之後，可以按一下按鈕即可透過分頁 DataList 的記錄。 請花一點時間現在就試試看 ！

## <a name="disabling-paging-interface-controls"></a>停用分頁介面控制項

目前，所有的四個按鈕會啟用不論要檢視的網頁。 不過，我們想要停用 [第一個和上一步] 按鈕時顯示的第一頁的資料，以及下一步] 和 [最後一個按鈕時顯示的最後一頁。 `PagedDataSource` ObjectDataSource s 所傳回物件`Select()`方法具有屬性[ `IsFirstPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx)和[ `IsLastPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) ，我們可以檢查來判斷是否我們目前檢視資料的第一個或最後一頁。

加入下列 ObjectDataSource s`Selected`事件處理常式：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

此步驟中，第一個和上一步 按鈕會停用檢視第一頁中，檢視最後一頁時，會停用下一步 和 最後一個按鈕時。

可讓 s 完成分頁介面通知使用者頁面它們目前正在檢視和總頁數存在 re。 將標籤 Web 控制項加入頁面並設定其`ID`屬性`CurrentPageNumber`。 設定其`Text`ObjectDataSource 的選取的事件處理常式這類屬性，它會包含目前正在檢視的頁面 (`PageIndex + 1`) 和總頁數 (`PageCount`)。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

圖 10 顯示`Paging.aspx`當第一次瀏覽。 查詢字串是空的因為在 DataList 會預設為顯示的前四個產品。第一個和上一步 按鈕會停用。 按一下 [下一步] 會顯示四個資料錄 （請參閱圖 11）;現在已啟用 [第一個和上一步] 按鈕。


[![第一個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**圖 10**: 第一個頁面的資料會顯示 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![第二個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**圖 11**: 第二個頁面的資料會顯示 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> 藉由允許使用者指定多少頁面來檢視每個頁面，可以進一步加強分頁介面。 例如，DropDownList 也可以加入像 5、 10、 25、 50 和所有的清單頁面大小選項。 在選取的頁面大小，使用者必須重新導向回到`Paging.aspx?pageIndex=0&pageSize=selectedPageSize`。 我將保留做為練習實作這項增強功能，讀取器。


## <a name="using-custom-paging"></a>使用自訂分頁

透過使用沒有效率的預設分頁技術其資料 DataList 頁面。 若充分大量的資料進行分頁，務必使用自訂分頁。 雖然稍有不同的實作詳細資料，在系統中實作自訂分頁的基本概念都相同做為使用預設分頁。 使用自訂分頁時，使用`ProductBLL`類別 s`GetProductsPaged`方法 (而不是`GetProductsAsPagedDataSource`)。 中所述[有效率地分頁透過大型量的資料](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教學課程中，`GetProductsPaged`必須傳遞開始資料列索引，最大值的資料列數目傳回。 這些參數可以透過查詢字串與進行維護`pageIndex`和`pageSize`所使用的參數在預設分頁。

因為沒有 s 沒有`PagedDataSource`使用自訂分頁時，必須使用替代技術來判斷透過及是否正在呼叫的記錄總數我們重新顯示資料的第一個或最後一頁。 `TotalNumberOfProducts()`方法中的`ProductsBLL`類別會傳回正在透過呼叫產品的總數。 檢查來判定是否檢視資料的第一頁，開始的資料列索引為零，則為正在檢視的第一頁。 檢視的最後一頁時是否起始資料列索引，再加上要傳回的最大資料列大於或等於透過正在呼叫的記錄總數。

我們將探討更詳細地實作自訂分頁，在下一個教學課程。

## <a name="summary"></a>總結

而 DataList 都中繼器提供的方塊中的分頁支援找到在 GridView 中 DetailsView，和 FormView 控制，這類功能可以加入最少的工作。 最簡單的方式來實作預設分頁是包裝的產品，在整組`PagedDataSource`然後再繫結`PagedDataSource`DataList 或重複項。 在本教學課程中，我們新增`GetProductsAsPagedDataSource`方法`ProductsBLL`類別，以傳回`PagedDataSource`。 `ProductsBLL`類別已經包含所需的自訂分頁方法`GetProductsPaged`和`TotalNumberOfProducts`。

以及擷取精確的設定要顯示的自訂分頁的記錄，或是中的所有記錄`PagedDataSource`為了預設分頁時，我們也必須手動新增分頁介面。 此教學課程中我們建立下一個、 上一步，首先，上次介面具有四個按鈕 Web 控制項。 此外，已加入標籤控制項，顯示目前的頁碼與總頁數。

在下一個教學課程中我們會看到如何將排序支援新增至 DataList 和中繼器。 我們也會看到如何建立可同時分頁和排序 （附範例使用預設和自訂分頁） 資料清單。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Liz Shulok、 Ken Pespisa 和 Bernadette Leigh。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一步](sorting-data-in-a-datalist-or-repeater-control-cs.md)
