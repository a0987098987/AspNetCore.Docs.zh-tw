---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: 分頁報表資料 DataList 或 Repeater 控制項 (C#) |Microsoft Docs
author: rick-anderson
description: 當收錄 DataList 或 Repeater 的供應項目自動分頁或排序支援，本教學課程會示範如何將分頁支援新增至 DataList 或 Repeater...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f77c4f781ee6001cea065d848f0a21a6cd4d7569
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401931"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>DataList 或 Repeater 控制項 (C#) 中的分頁報表資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe)或[下載 PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> 雖然收錄 DataList 或 Repeater 的供應項目自動分頁或排序支援，本教學課程會示範如何將分頁支援新增至 DataList 或 Repeater，允許更有彈性的分頁及資料顯示介面。


## <a name="introduction"></a>簡介

分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。 比方說，搜尋時在線上書店的 ASP.NET 書籍，可能有數百個這類的書籍，但報告，列出搜尋結果會列出每頁只十個相符項目。 此外，結果可以依照標題、 價格、 頁數、 作者名稱等等。 如我們所述[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教學課程中，所有提供內建的刻度的核取方塊可以啟用的分頁支援的 GridView、 DetailsView 和 FormView 控制項。 GridView 也包含排序支援。

不幸的是，DataList 和 Repeater 都不會提供自動分頁或排序支援。 在本教學課程中，我們將檢驗如何將分頁支援新增至 DataList 或 Repeater。 我們必須以手動方式建立分頁介面顯示的記錄，以適當的頁面，請記得在回傳間所造訪的頁面。 雖然這需要更多的時間和程式碼比使用 GridView、 DetailsView 或 FormView，DataList 與重複項允許更有彈性的分頁及資料顯示介面。

> [!NOTE]
> 本教學課程著重在分頁。 在下一個教學課程中我們將把焦點轉到新增排序功能。


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步驟 1： 加入分頁和排序教學課程的網頁

在開始本教學課程之前，可讓 s 先花點時間，將 ASP.NET 頁面，我們需要針對本教學課程中，另一個。 藉由建立新的資料夾中名為專案啟動`PagingSortingDataListRepeater`。 接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![建立 PagingSortingDataListRepeater 資料夾，並新增這些教學課程的 ASP.NET 網頁](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**圖 1**： 建立`PagingSortingDataListRepeater`資料夾並新增教學課程的 ASP.NET 頁面


接下來，開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。 此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並顯示這些教學課程中的項目符號清單中目前的區段。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


若要有項目符號清單顯示分頁和排序我們將建立的教學課程，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並在編輯和刪除之後新增下列標記，以 DataList 站台對應節點標記：


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁


## <a name="a-review-of-paging"></a>分頁的檢閱

在先前的教學課程中，我們了解如何透過 GridView、 DetailsView 和 FormView 控制項中的資料頁面上的內容。 這三個控制項提供簡單的呼叫中的分頁*預設分頁*可以實作只要選取 控制項 s 智慧標籤中啟用分頁的選項。 使用預設分頁時，每次要求的資料頁時的第一頁上瀏覽，或當使用者瀏覽至其他頁面的資料 GridView、 DetailsView 或 FormView 控制項重新要求*所有*的資料ObjectDataSource。 它然後剪取時顯示的資料錄的特定集給定要求的頁面索引和每頁顯示的記錄數目。 我們討論過詳細的預設分頁[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教學課程。

預設的分頁重新要求每個頁面的所有記錄，因為它並不實用時夠大量的資料進行分頁。 例如，假設透過頁面大小為 10 的 50,000 筆記錄的分頁。 每當使用者移到新的頁面上，必須擷取從資料庫中，所有的 50,000 筆記錄，即使唯一的十個，其中會顯示。

*自訂分頁*解決效能問題的預設分頁擷取要求的頁面上顯示的資料錄的精確子集。 在實作自訂分頁時，我們必須撰寫 SQL 查詢，有效率地將會傳回只是一組正確的記錄。 我們了解如何建立使用新的 SQL Server 2005 s 這類查詢[`ROW_NUMBER()`關鍵字](http://www.4guysfromrolla.com/webtech/010406-1.shtml)回到[有效率地分頁透過大型數量的資料](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教學課程。

DataList 或 Repeater 控制項中實作預設的分頁功能，我們可以使用[`PagedDataSource`類別](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx)周圍的包裝函式`ProductsDataTable`是否正在分頁的內容。 `PagedDataSource`類別具有`DataSource`可以指派給任何可列舉物件的屬性和[ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx)並[ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx)指出多少筆記錄的屬性顯示每個頁面和目前的頁面索引。 一旦有尚未設定這些屬性，`PagedDataSource`可用來當做資料來源的任何資料 Web 控制項。 `PagedDataSource`、 列舉時，會唯一傳回的記錄其內部的適當子集`DataSource`根據`PageSize`和`CurrentPageIndex`屬性。 圖 4 說明的功能`PagedDataSource`類別。


![PagedDataSource 包裝的可分頁介面的可列舉物件](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**圖 4**:`PagedDataSource`包裝的可分頁介面的可列舉物件


`PagedDataSource`物件可以是建立和設定直接從商務邏輯層和繫結至 DataList 或 Repeater 透過 ObjectDataSource，或可以建立及設定直接在 ASP.NET 頁面 s 程式碼後置類別中。 如果第二種方法，就必須放棄使用 ObjectDataSource，並改為將資料繫結分頁至 DataList 或 Repeater 以程式設計的方式。

`PagedDataSource`物件也具有屬性，以支援自訂分頁。 不過，我們可以略過使用`PagedDataSource`用於自訂分頁，因為我們已經有 BLL 方法`ProductsBLL`類別專為傳回精確的記錄顯示的自訂分頁。

在本教學課程將探討在 DataList 中實作預設的分頁功能，藉由新增新的方法來`ProductsBLL`會傳回已適當設定的類別`PagedDataSource`物件。 在下一個教學課程中，我們會看到如何使用自訂分頁。

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>步驟 2： 新增商務邏輯層中的預設分頁方法

`ProductsBLL`類別目前已傳回產品的所有資訊的方法`GetProducts()`，另一個用於都傳回起始的索引處的產品的特定子集`GetProductsPaged(startRowIndex, maximumRows)`。 使用預設分頁 GridView、 DetailsView 和 FormView 控制項所有使用`GetProducts()`方法來擷取所有的產品，但接著使用`PagedDataSource`在內部以顯示記錄中的正確子集合。 若要複寫使用 DataList 與重複項控制項的這項功能，我們可以模擬此行為到 BLL 中建立的新方法。

將方法加入`ProductsBLL`名為類別`GetProductsAsPagedDataSource`會採用兩個整數輸入參數：

- `pageIndex` 若要顯示，頁面的索引編製索引，位於零，並
- `pageSize` 每頁顯示的記錄數目。

`GetProductsAsPagedDataSource` 首先會擷取*所有*記錄的`GetProducts()`。 然後它會建立`PagedDataSource`物件，設定其`CurrentPageIndex`並`PageSize`屬性，以傳入的值`pageIndex`和`pageSize`參數。 此方法結束時，會傳回設定`PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>步驟 3： 使用的預設分頁 DataList 中顯示產品資訊

具有`GetProductsAsPagedDataSource`方法新增至`ProductsBLL`類別中，我們現在可以建立 DataList 或 Repeater 提供預設的分頁功能。 首先開啟`Paging.aspx`頁面中`PagingSortingDataListRepeater`資料夾，然後從 [工具箱] 拖曳至設計工具，設定 DataList 的拖曳 DataList`ID`屬性設`ProductsDefaultPaging`。 從 DataList s 智慧標籤，建立名為新 ObjectDataSource`ProductsDefaultPagingDataSource`並將其設定，因此，它會擷取使用資料`GetProductsAsPagedDataSource`方法。


[![建立 ObjectDataSource，並將它設定為使用 GetProductsAsPagedDataSource （） 方法](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**圖 5**： 建立 ObjectDataSource，並將它設定為使用`GetProductsAsPagedDataSource``()`方法 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


設定下拉式清單中更新、 插入和刪除 （無） 索引標籤。


[![設定下拉式清單中更新、 插入和刪除索引標籤為 （無）](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**圖 6**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


因為`GetProductsAsPagedDataSource`方法需要兩個輸入的參數，此精靈會提示我們輸入這些參數值的來源。

頁面索引和頁面大小值必須記住在回傳之間。 它們可以儲存在檢視狀態、 保存至查詢字串，儲存在工作階段變數，或記住使用一些其他技術。 在本教學課程中，我們將使用查詢字串，好處是允許的資料會設為書籤的特定頁面。

特別是，使用 查詢字串欄位 pageIndex 和的 pageSize`pageIndex`和`pageSize`參數，分別 （請參閱 圖 7）。 為贏得 t 的查詢字串值時必須存在使用者第一次瀏覽此頁面，請花一點時間設定這些參數的預設值。 針對`pageIndex`，設定的預設值為 0 （這會顯示資料的第一頁） 和`pageSize`的預設值為 4。


[![PageIndex 和 pageSize 參數作為來源的查詢字串](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**圖 7**： 使用查詢字串做為來源`pageIndex`並`pageSize`參數 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


設定之後 ObjectDataSource，Visual Studio 會自動建立`ItemTemplate`的 DataList。 自訂`ItemTemplate`以便顯示只有 s 產品名稱、 類別和供應商。 也設定 DataList s`RepeatColumns`屬性設為 2，其`Width`為 100%，並將其`ItemStyle`s`Width`為 50%。 這些寬度設定會提供兩個資料行的相等間距。

進行這些變更之後，DataList 與 ObjectDataSource 的標記看起來應該如下所示：


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> 因為我們不會執行任何更新或刪除在本教學課程中的功能，您可能會停用 DataList s 的檢視狀態，以減少呈現的頁面大小。


當一開始瀏覽此頁面在瀏覽器都`pageIndex`也不`pageSize`提供查詢字串參數。 因此，會使用 0 到 4 的預設值。 如 [圖 8] 所示，這會導致顯示前四個產品的 DataList。


[![第四個產品詳列](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**圖 8**： 列出的第四個產品 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


沒有使用者瀏覽至第二個資料頁，表示分頁介面，該處 s 目前無法直接了當。 我們將在步驟 4 中建立的分頁介面。 現在，不過，分頁僅可藉由直接於 querystring 中指定的分頁準則。 例如，若要檢視的第二頁，變更瀏覽器的網址列中的 URL`Paging.aspx`至`Paging.aspx?pageIndex=2`按 enter 鍵。 這會導致要顯示資料的第二頁 （請參閱 圖 9）。


[![第二個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**圖 9**： 會顯示第二個頁面的資料 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>步驟 4： 建立分頁介面

有各種不同的分頁介面可以實作。 GridView、 DetailsView 和 FormView 控制項提供四個不同的介面，可選擇：

- **接下來，先前**使用者可以在階段中的一個或下一個一個移動一頁。
- **下一步 上, 一步中，先上次**除了下一步 和 上一頁 按鈕，這個介面包含將移至第一個或最後一個頁面的第一個和最後一個按鈕。
- **數值**列出頁面中的數字的分頁介面，讓使用者快速跳至特定頁面。
- **數值，第一次，最後一個**數值的頁碼，除了包含 移至第一個或最後一個頁面上的按鈕。

DataList 和 Repeater，我們會負責決定分頁介面，並實作它。 這牽涉到頁面中建立所需的 Web 控制項和顯示要求的頁面，按一下特定的分頁介面按鈕時。 此外，某些分頁介面控制項可能需要停用。 比方說，當您檢視使用下一個資料的第一頁上, 一步，第一次，最後一個介面，第一個] 和 [上一步按鈕就會停用。

本教學課程中，讓的使用下一步，在上一步，第一次，最後的介面。 將四個按鈕 Web 控制項加入頁面，並設定其`ID`s `FirstPage`， `PrevPage`， `NextPage`，和`LastPage`。 設定`Text`屬性，以&lt;&lt;第一， &lt; Prev、 下一步&gt;，和最後一個&gt; &gt; 。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

接下來，建立`Click`鎯瓾餂鈕的每個事件處理常式。 稍後我們將新增顯示要求的頁面所需的程式碼。

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>記住透過正在分頁的記錄總數

不論選取的分頁介面，我們必須計算，並請記得透過正在分頁的記錄總數。 總計資料列中的計數 （搭配頁面大小） 決定多少的總頁數的資料正在分頁，以決定哪些分頁介面控制項加入或已啟用。 在下一步 上, 一步，第一個，最後一個介面，我們要建置的頁面計數用於兩種方式：

- 若要判斷是否我們正在檢視的最後一頁，在此情況下會停用下一步] 和 [最後一個按鈕。
- 如果使用者按一下最後一個按鈕，我們要去 whisk 它們小於頁面的頁面上，其索引是其中一個計數。

除以頁面大小的資料列總數的上限被計算頁面計數。 比方說，如果我們會透過 79 記錄與每一頁的四個記錄的分頁，則頁面計數是 20 (79 上限 / 4)。 如果我們使用的數字的分頁介面，這項資訊就會將我們通知有關要顯示; 的幾個數值頁面按鈕如果我們的分頁介面包含下一個或最後一個按鈕，頁面計數用來判斷何時要停用下一步] 或 [最後一個按鈕。

如果分頁介面將包含最後一個按鈕，請務必透過正在分頁的記錄總數會記住在回傳之間，以便按一下最後一個按鈕時，我們就可以判斷最後一個頁面索引。 若要達成此目的，建立`TotalRowCount`保存它的值，來檢視狀態的 ASP.NET 頁面 s 程式碼後置類別中的屬性：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

除了`TotalRowCount`、 花點時間建立唯讀頁面層級屬性，以輕鬆存取的頁面索引、 頁面大小和頁面計數：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>判斷透過正在分頁的記錄總數

`PagedDataSource`物件傳回從 ObjectDataSource s`Select()`其內的方法有*所有*一個產品記錄，即使它們的子集會顯示在資料清單。 `PagedDataSource` s [ `Count`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx)會傳回只會顯示 DataList; 中的項目數目[`DataSourceCount`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx)傳回內的項目總數`PagedDataSource`. 因此，我們需要將 ASP.NET 頁面 s`TotalRowCount`屬性值的`PagedDataSource`s`DataSourceCount`屬性。

若要達成此目的，建立事件處理常式 ObjectDataSource s`Selected`事件。 在 `Selected`我們可以存取的 ObjectDataSource s 的傳回值的事件處理常式`Select()`方法，在此情況下， `PagedDataSource`。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>顯示資料的要求的頁面

當使用者按一下其中一個按鈕，在分頁介面中時，我們要顯示的資料要求的頁面。 因為分頁參數會透過查詢字串，指定要顯示的資料，則使用要求的頁面`Response.Redirect(url)`有使用者 s 瀏覽器重新要求`Paging.aspx`以適當的分頁參數 頁面。 例如，若要顯示資料的第二頁，我們會將使用者重新導向至`Paging.aspx?pageIndex=1`。

若要達成此目的，建立`RedirectUser(sendUserToPageIndex)`若要將使用者重新導向的方法`Paging.aspx?pageIndex=sendUserToPageIndex`。 然後，呼叫這個方法，從四個按鈕`Click`事件處理常式。 在  `FirstPage` `Click`事件處理常式中，呼叫`RedirectUser(0)`，以將它們傳送至第一頁，在`PrevPage``Click`事件處理常式，使用`PageIndex - 1`做為頁面的索引;，依此類推。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

使用`Click`事件處理常式完成，DataList 的記錄可以透過分頁，按一下按鈕。 花點時間，現在就試試看 ！

## <a name="disabling-paging-interface-controls"></a>停用分頁介面控制項

目前，所有的四個按鈕會啟用不論要檢視的網頁。 不過，我們想要顯示的第一頁的資料，以及下一步] 和 [最後一個按鈕時顯示的最後一頁時，停用 [第一個和上一步] 按鈕。 `PagedDataSource` ObjectDataSource s 所傳回的物件`Select()`方法都有屬性[ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx)並[ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) ，我們可以檢查來判斷是否我們目前檢視資料的第一個或最後一頁。

將下列內容新增到 ObjectDataSource`Selected`事件處理常式：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

此步驟中，[第一個和上一步] 按鈕會停用檢視的第一頁，檢視的最後一頁時，會停用下一步] 和 [最後一個按鈕時。

可讓 s 完成分頁介面通知使用者項目頁面上它們目前正在檢視和多少的總頁數存在 re。 將標籤 Web 控制項新增至頁面並設定其`ID`屬性設`CurrentPageNumber`。 設定其`Text`ObjectDataSource 的選取的事件處理常式這類屬性，它會包含目前正在檢視的頁面 (`PageIndex + 1`) 和總頁數 (`PageCount`)。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

[圖 10] 顯示`Paging.aspx`當第一次瀏覽。 查詢字串是空的因為 DataList 會預設為顯示前四個產品中;[第一個和上一步] 按鈕會停用。 按一下 下一步 會顯示接下來四個資料錄 （請參閱 圖 11）;現在已啟用 第一個和上一步 按鈕。


[![第一個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**圖 10**： 會顯示第一個頁面的資料 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![第二個頁面的資料會顯示](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**圖 11**： 會顯示第二個頁面的資料 ([按一下以檢視完整大小的影像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> 允許使用者指定多少頁面來檢視每個頁面，可以進一步增強分頁介面。 比方說，DropDownList 也可以加入像 5、 10、 25、 50 和所有的清單頁面大小選項。 在選取的頁面大小，使用者必須重新導向回到`Paging.aspx?pageIndex=0&pageSize=selectedPageSize`。 我保留讀取器實作，做練習這項增強功能。


## <a name="using-custom-paging"></a>使用自訂分頁

透過使用效率不佳的預設分頁技術及其資料 DataList 頁面。 當夠大量的資料進行分頁，務必使用自訂分頁。 雖然稍有不同的實作詳細資料，DataList 中實作自訂分頁背後的概念並使用預設分頁時相同。 使用自訂分頁時，使用`ProductBLL`類別 s`GetProductsPaged`方法 (而不是`GetProductsAsPagedDataSource`)。 中所述[有效率地分頁透過大型數量的資料](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教學課程中，`GetProductsPaged`必須傳遞開始的資料列索引和最大值的資料列數目傳回。 這些參數可以透過查詢字串就如同進行維護`pageIndex`和`pageSize`參數使用的預設分頁。

因為沒有 s 沒有`PagedDataSource`使用自訂分頁時，必須使用替代技術來判斷透過及是否正在分頁的記錄總數我們重新顯示資料的第一個或最後一個頁面。 `TotalNumberOfProducts()`方法中的`ProductsBLL`類別會傳回正在分頁透過產品的總數。 若要判斷是否要檢視資料的第一頁，檢查起始資料列索引為零，則在檢視的第一頁。 如果起始資料列索引，再加上要傳回的最大資料列是大於或等於透過正在分頁的記錄總數，是正在檢視的最後一頁。

我們將探討在下一個教學課程中，更詳細地實作自訂分頁。

## <a name="summary"></a>總結

而 DataList 和 Repeater 都不提供現成的方塊中的分頁支援中找到 GridView、 DetailsView 和 FormView 控制項，可以新增這類功能，而最少的工作。 實作預設的分頁功能最簡單的方式是將包裝產品內的整組`PagedDataSource`然後再繫結`PagedDataSource`DataList 或 Repeater。 在本教學課程中，我們新增`GetProductsAsPagedDataSource`方法，以`ProductsBLL`類別，以傳回`PagedDataSource`。 `ProductsBLL`類別已包含的方法所需的自訂分頁`GetProductsPaged`和`TotalNumberOfProducts`。

擷取是一組精確自訂分頁顯示的資料錄或中的所有記錄，以及`PagedDataSource`的預設分頁時，我們也需要手動新增分頁介面。 本教學課程中我們建立下一步、 上一步，第一次，最後一個介面具有四個按鈕 Web 控制項。 此外，已新增一個 Label 控制項，顯示目前的頁碼和總頁數。

在下一個教學課程中，我們會看到如何將排序支援新增至 DataList 與重複項。 我們也會看到如何建立可同時分頁和排序 （附範例使用預設和自訂分頁） DataList。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Liz Shulok、 Ken Pespisa 和 Bernadette Leigh。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](sorting-data-in-a-datalist-or-repeater-control-cs.md)
