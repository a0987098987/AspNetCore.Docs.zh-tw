---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: 建立自訂的排序使用者介面 (VB) |Microsoft Docs
author: rick-anderson
description: 當顯示一長串的已排序的資料時，它可以是非常有幫助藉由引進分隔符號資料列群組相關的資料。 在本教學課程中我們將看到如何建立...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e94bbda0ca89b409515db1e223637e3a554cd31
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835864"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>建立自訂的排序使用者介面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe)或[下載 PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> 當顯示一長串的已排序的資料時，它可以是非常有幫助藉由引進分隔符號資料列群組相關的資料。 在本教學課程中，我們將了解如何建立這類排序的使用者介面。


## <a name="introduction"></a>簡介

當顯示一長串的已排序的資料有只有少數幾個不同的值已排序的資料行中，使用者可能會覺得很難分辨，完全差異界限發生。 比方說，有 81 的產品，在資料庫中，但只有九個不同的類別選擇 (八個唯一的類別目錄再加上`NULL`選項)。 請考慮檢查落在 Seafood 類別目錄的產品有興趣的使用者的情況。 從列出的頁面*所有*單一 GridView 中的產品，使用者可能會決定她的最佳選擇是以排序結果依類別目錄，將會分組在一起的所有 Seafood 產品放在一起。 完成排序之後的類別目錄，然後使用者必須四處整個清單，尋找 Seafood 分組產品的開頭與結尾。 因為結果會依字母順序排序尋找 Seafood 產品類別目錄名稱並不困難，但仍需要仔細掃描在方格中的項目清單。

為了協助反白顯示已排序的群組之間的界限，許多網站會採用使用者介面，將這類群組之間的分隔符號。 像 [圖 1] 所示的分隔頁可讓使用者更快速地找到特定的群組和識別它的界限，以及確定有哪些不同的群組會存在於資料。


[![每個類別目錄群組是清楚識別](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**圖 1**： 每個類別目錄群組是清楚識別 ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


在本教學課程中，我們將了解如何建立這類排序的使用者介面。

## <a name="step-1-creating-a-standard-sortable-gridview"></a>步驟 1： 建立標準、 可排序的 GridView

我們將探討如何增強 GridView，以提供增強的排序介面之前，讓第一次建立標準、 可排序的 GridView，列出的產品。 首先開啟`CustomSortingUI.aspx`頁面中`PagingAndSorting`資料夾。 新增 GridView 的頁面，將其`ID`屬性設`ProductList`，並將它繫結至新的 ObjectDataSource。 設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProducts()`選取記錄方法。

接下來，設定 GridView，使其只包含`ProductName`， `CategoryName`， `SupplierName`，和`UnitPrice`BoundFields 及其已停止。 最後，設定 GridView，以支援排序的 GridView s 智慧標籤的 啟用排序核取方塊 (或藉由設定其`AllowSorting`屬性設`true`)。 進行這些新增項目後`CustomSortingUI.aspx` 頁面上，宣告式標記看起來應該如下所示：


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

請花一點時間到目前為止在瀏覽器中檢視進度。 圖 2 顯示其資料依分類依字母順序排序時排序 GridView。


[![可排序的 GridView s 類別目錄排序資料](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**圖 2**： 依類別排序資料的可排序 GridView s ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>步驟 2： 針對分隔符號資料列加入探索技術

使用泛型、 可排序 gridview 裡完成，所有剩下就是要能夠在每個唯一的已排序群組之前 gridview 裡加入分隔符號資料列。 但是，如何可以這類資料列插入至 GridView 嗎？ 基本上，我們需要逐一查看的 GridView s 資料列中，判斷發生的已排序的資料行中值之間的差異，然後加入適當的分隔符號資料列。 當考慮這個問題，似乎自然解決方案介於兩者 GridView 的`RowDataBound`事件處理常式。 如我們所述[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-vb.md)教學課程中，此事件處理常式通常會套用資料列層級格式化以資料列的資料為基礎。 不過，`RowDataBound`事件處理常式不是這個問題的解決方案，資料列無法從這個事件處理常式以程式設計方式新增至 GridView。 GridView 的`Rows`集合，實際上是唯讀狀態。

若要將額外的資料列新增至 GridView 中，我們有三種選擇：

- 這些中繼資料的分隔符號資料列加入繫結至 GridView 的實際資料
- GridView 有繫結至資料之後，新增其他`TableRow`執行個體到 GridView 控制項集合
- 建立自訂伺服器控制項擴充 GridView 控制項，會覆寫這些方法負責建構 GridView 的結構

如果這項功能需要許多網頁上或跨多個網站，建立自訂伺服器控制項將會最好的方法。 不過，它會需要相當多的程式碼和 GridView s 內部運作必需品到完整探索。 因此，我們將不考慮該選項，在本教學課程。

其他兩個選項至實際資料正在加入分隔符號資料列繫結至 GridView 和操作 GridView 的控制項集合之後, 它已繫結-攻擊問題以不同的方式和值得討論。

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>將資料列加入資料繫結至 GridView

GridView 繫結至資料來源，它會建立`GridViewRow`資料來源傳回每一筆記錄。 因此，我們可以將所需的分隔符號資料列加入至資料來源的分隔記錄之前繫結至 GridView。 圖 3 闡明此概念。


![其中一種技術牽涉到將分隔符號資料列加入至資料來源](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**圖 3**： 其中一種技術牽涉到將分隔符號資料列加入至資料來源


我將使用的詞彙分隔符號記錄以引號括住，因為沒有特殊分隔符號記錄;相反地，我們必須以某種方式的旗標做為分隔符號，而不是一般的資料列的資料來源中的特定記錄。 如需我們的範例，我們重新繫結`ProductsDataTable`執行個體至 GridView，這組成`ProductRows`。 我們可能會加上旗標一筆記錄的分隔符號資料列藉由設定其`CategoryID`屬性設`-1`（因為這類的值無法 t 通常存在）。

若要利用這項技術 d 我們需要執行下列步驟：

1. 以程式設計方式擷取的資料繫結至 GridView (`ProductsDataTable`執行個體)
2. 排序 GridView s 所依據的資料`SortExpression`和`SortDirection`屬性
3. 逐一查看`ProductsRows`在`ProductsDataTable`，尋找已排序的資料行中的差異，位於
4. 在每個路由群組間界限，插入分隔資料錄`ProductsRow`DataTable，其中有它 s 執行個體`CategoryID`設定為`-1`（或任何指定已決定將標示為分隔記錄的記錄）
5. 之後插入分隔符號資料列，以程式設計方式將資料繫結至 GridView

除了這五個步驟中，我們的 d 也必須提供事件處理常式 GridView s`RowDataBound`事件。 在這裡，我們的 d 檢查每個`DataRow`，並判斷它是否為分隔符號，第一列其`CategoryID`設定在當時`-1`。 如果是這樣，我們的 d 可能會想要調整其格式設定] 或 [資料格所顯示的文字。

插入排序群組界限在您需要也提供如 GridView s 的 事件處理常式，需要更多的工作，比上述，使用這項技術`Sorting`事件，並保留軌`SortExpression`和`SortDirection`值。

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>操作 GridView s 控制 s 後的集合已資料繫結

而不是訊息之前繫結至 GridView 的資料，我們可以加入分隔符號資料列*之後*資料繫結至 GridView。 資料繫結的程序會建立 GridView 的控制階層架構，這實際上是只要`Table`執行個體所組成的資料列，每一個都儲存格的集合所組成的集合。 具體來說，GridView 的控制項集合包含`Table`物件，其根目錄`GridViewRow`(衍生自`TableRow`類別) 中的每一筆記錄`DataSource`繫結至 GridView，和`TableCell`物件中每個`GridViewRow`執行個體中每個資料欄位`DataSource`。

若要新增每個排序的群組之間的分隔符號資料列，我們可以直接操作此控制項階層架構一旦建立之後。 我們可以確信 GridView 的控制項階層架構已經建立的最後一次呈現網頁時的時間。 因此，這個方法會覆寫`Page`類別的`Render`方法，此時 GridView s 最終的控制項階層架構會更新以包含所需的分隔符號資料列。 [圖 4] 說明此程序。


[![這項替代技術操作 GridView 的控制項階層架構](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**圖 4**： 這項替代技術操作 s 的 GridView 控制項階層架構 ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


本教學課程中，我們將使用此第二種方法，自訂排序的使用者體驗。

> [!NOTE]
> 程式碼我 m 呈現在本教學課程根據提供的範例[Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s 部落格項目[播放 GridView 排序分組的位元](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx)。


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>步驟 3： 分隔符號資料列加入至 GridView 的控制項階層架構

因為我們只想要將分隔符號資料列新增至 GridView 的控制項階層架構，已建立並在該頁面瀏覽的最後一次建立其控制項階層架構之後，我們想要執行此新增結尾的頁面生命週期中，但在實際的 GridView c 之前下時階層已呈現成 HTML。 最新的可能點，此時我們可以完成這項作業是`Page`類別的`Render`事件，我們可以在我們使用下列方法簽章的程式碼後置類別中覆寫：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

當`Page`類別 s 原始`Render`叫用方法`base.Render(writer)`每個網頁中的控制項將會呈現，產生根據其控制項階層架構的標記。 因此務必，我們都呼叫`base.Render(writer)`，如此一來，呈現網頁時，我們操作 GridView s 和控制階層架構，才能呼叫`base.Render(writer)`，如此分隔符號資料列已加入至之前的 GridView 的控制項階層架構s 已轉譯。

若要插入排序群組標頭，我們必須先確定使用者已要求的資料進行排序。 根據預設，不會排序 GridView 的內容，，並因此我們不 t 不必輸入排序的標頭的任何群組。

> [!NOTE]
> 如果您想在第一次載入頁面時，會依照特定的資料行的 GridView，呼叫 GridView 的`Sort`方法在第一個頁面造訪 （但不是會在後續回傳時）。 若要達成此目的，將中的此呼叫`Page_Load`內的事件處理常式`if (!Page.IsPostBack)`條件。 回頭[分頁和排序報表資料](paging-and-sorting-report-data-vb.md)教學課程的資訊，如需詳細資訊`Sort`方法。


假設資料已排序下, 一步是要判斷哪一個資料行資料已排序，然後可以掃描尋找該資料行 s 中的差異資料列的值。 下列程式碼，以確保資料的已排序和尋找資料已排序的資料行：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

如果 GridView 有尚未要排序，GridView 的`SortExpression`將尚未設定屬性。 因此，我們只想要新增的分隔符號資料列，如果此屬性的某些值。 若是如此，我們接下來要決定資料已排序依據的資料行的索引。 這透過循環使用 GridView s`Columns`集合，其搜尋資料行`SortExpression`屬性等於 GridView 的`SortExpression`屬性。 除了資料行的索引中，我們也抓取`HeaderText`屬性，這會顯示分隔符號資料列時。

資料已排序的資料行索引，最後一個步驟是列舉的 GridView 資料列。 每個資料列中，我們必須決定是否排序資料行的值不同於先前的資料列排序的 s 資料行的值。 因此，我們需要將新如果`GridViewRow`到的控制項階層架構的執行個體。 這是由下列程式碼來完成：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

此程式碼一開始會以程式設計方式參考`Table`物件的 GridView 的控制項階層架構的根目錄中找到，並建立名為的字串變數`lastValue`。 `lastValue` 用來比較目前資料列排序的 s 資料行的值與先前的資料列的值。 接下來，在 GridView s`Rows`會列舉集合，而且每個資料列已排序的資料行的值會儲存在`currentValue`變數。

> [!NOTE]
> 若要判斷特定資料列排序的 s 資料行的值中，我使用的儲存格的`Text`屬性。 這適用於 BoundFields，但不是會運作所需的 TemplateFields，CheckBoxFields，和等等。 我們將探討如何稍後說明替代的 GridView 欄位。


`currentValue`和`lastValue`變數進行比較。 如果它們之間的差異我們需要的控制項階層架構中加入新的分隔符號資料列。 這可以藉由判斷索引`GridViewRow`中`Table`物件 s`Rows`集合中，建立新`GridViewRow`並`TableCell`執行個體，然後再新增`TableCell`和`GridViewRow`至控制階層架構。

分隔符號資料列 s 單獨的附註`TableCell`會格式化，該跨越整個寬度的 gridview 裡，會使用格式化`SortHeaderRowStyle`CSS 類別，且其`Text`滿足下列條件，它會顯示這兩個排序群組名稱 （例如類別） 的屬性和（例如飲料） 群組的值。 最後，`lastValue`已更新的值為`currentValue`。

用來格式化排序的群組標頭資料列的 CSS 類別`SortHeaderRowStyle`必須在指定`Styles.css`檔案。 可以自由使用任何的樣式設定吸引您;我使用下列項目：


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

使用目前的程式碼中，排序的介面新增排序群組標頭排序任何 BoundField 時 （請參閱 圖 5，供應商所排序時，顯示螢幕擷取畫面）。 不過，排序時 （例如 CheckBoxField 或 TemplateField） 任何其他欄位型別，排序群組標頭是沒地方可以找到 （請參閱 圖 6）。


[![排序介面 BoundFields 排序時，請包含排序群組標頭](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**圖 5**: 排序介面包含排序群組標頭時排序 BoundFields ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![排序群組標頭會遺漏當排序 CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**圖 6**: 排序群組標頭會遺漏當排序 CheckBoxField ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


CheckBoxField 排序時，會遺失排序群組標頭的原因是因為此程式碼目前使用剛`TableCell`s`Text`屬性來判斷每個資料列已排序的資料行的值。 CheckBoxFields，如`TableCell`s`Text`屬性為空字串; 相反地，值是透過核取方塊 Web 控制項內`TableCell`s`Controls`集合。

我們需要來增強程式碼以處理 BoundFields 以外的欄位型別，其中`currentValue`已指派變數，以檢查是否存在的核取方塊`TableCell`s`Controls`集合。 而不是使用`currentValue = gvr.Cells(sortColumnIndex).Text`，此程式碼取代為下列：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

此程式碼會檢查已排序的資料行`TableCell`若要判斷是否有任何控制項中的目前資料列`Controls`集合。 如果有，而且第一個控制項的核取方塊，`currentValue`變數設為 是 或 否，根據核取方塊的`Checked`屬性。 否則，值取自`TableCell`s`Text`屬性。 這個邏輯可以處理排序的 GridView 中可能存在任何 TemplateFields 複寫。

上述程式碼加入之後，排序群組標頭現在都已停止的 CheckBoxField 排序時 （請參閱 圖 7）。


[![排序群組標頭會立即出現時排序 CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**圖 7**: 排序群組標頭會立即出現時排序 CheckBoxField ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> 如果您有使用的產品`NULL`資料庫的值`CategoryID`， `SupplierID`，或`UnitPrice`欄位，這些值會顯示為 GridView 中的空字串的預設值，表示分隔符號 s 的資料列文字的那些產品之`NULL`值將讀取像類別目錄: (也就是沒有 s 之後類別沒有名稱： 例如類別目錄： 飲料)。 如果您想要顯示在這裡您可以設定 BoundFields [ `NullDisplayText`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx)文字您想要顯示或指派時，您可以在 Render 方法中新增的條件陳述式`currentValue`分隔符號資料列的`Text`屬性。


## <a name="summary"></a>總結

GridView 不包含許多內建的選項，可以自訂排序的介面。 不過，若使用的低層級的程式碼，它可以調整 GridView 的控制項階層架構，來建立更客製化的介面。 在本教學課程中，我們看到如何將排序群組分隔符號資料列加入一個可排序的 GridView，更輕鬆地識別不同的群組以及這些群組的界限。 如需的自訂排序介面的其他範例，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/) s[一些 ASP.NET 2.0 GridView 排序祕訣和訣竅](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx)部落格文章。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](sorting-custom-paged-data-vb.md)
