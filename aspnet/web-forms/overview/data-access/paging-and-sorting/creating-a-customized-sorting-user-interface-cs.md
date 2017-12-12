---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: "建立自訂排序的使用者介面 (C#) |Microsoft 文件"
author: rick-anderson
description: "當顯示一長串的已排序資料時，它可以是很有幫助藉由引進分隔符號資料列群組相關的資料。 在本教學課程中我們會看到如何建立..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: bc009272df3626402ee4c52578f9b364f70a4e78
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-customized-sorting-user-interface-c"></a>建立自訂排序的使用者介面 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe)或[下載 PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> 當顯示一長串的已排序資料時，它可以是很有幫助藉由引進分隔符號資料列群組相關的資料。 在本教學課程中，我們會看到如何建立這類排序的使用者介面。


## <a name="introduction"></a>簡介

顯示一長串的排序資料時有少數幾個不同的值已排序的資料行中，使用者可能會發現很難分辨，完全，差異界限發生。 例如，有在資料庫中，但只有 9 個不同的類別目錄選擇 81 產品 (八個唯一類別加上`NULL`選項)。 請檢查 Seafood 種類的產品感興趣的使用者。 從頁面，其中列出*所有*單一 GridView 中的產品，使用者可能會決定其最好是在排序依據類別、 群組在一起的結果所有 Seafood 產品放在一起。 完成排序之後的分類，然後使用者必須以搜尋整個清單，尋找其中的 Seafood 分組產品開始和結束。 因為結果會依字母順序排序尋找 Seafood 產品類別目錄名稱並不困難，但仍需要密切掃描的方格中的項目清單。

反白顯示已排序的群組之間的界限採用，以協助許多網站的使用者介面，將此類群組之間的分隔符號。 圖 1 所示的分隔頁可讓使用者更快速地尋找特定的群組和識別其界限，以及確定資料中存在哪些不同的群組。


[![每個類別目錄群組是清楚識別](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**圖 1**： 每個類別目錄群組是清楚識別 ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


在本教學課程中，我們會看到如何建立這類排序的使用者介面。

## <a name="step-1-creating-a-standard-sortable-gridview"></a>步驟 1： 建立標準、 可排序的 GridView

我們探討如何加強提供增強的排序介面 GridView 之前，可讓第一次建立列出產品的標準、 可排序 GridView。 先開啟`CustomSortingUI.aspx`頁面`PagingAndSorting`資料夾。 將 GridView 加入頁面，設定其`ID`屬性`ProductList`，並將它繫結至新 ObjectDataSource。 設定用於 ObjectDataSource`ProductsBLL`類別的`GetProducts()`選取記錄方法。

接下來，設定 GridView，使其只包含`ProductName`， `CategoryName`， `SupplierName`，和`UnitPrice`BoundFields 及其已停止。 最後，設定 GridView 支援排序，藉由檢查 GridView s 智慧標籤的 啟用排序核取方塊 (或藉由設定其`AllowSorting`屬性`true`)。 進行這些新增項目後`CustomSortingUI.aspx` 頁面上，宣告式標記看起來應該類似下列：


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

請花一點時間為止的瀏覽器中檢視進度。 圖 2 顯示可排序 GridView，其資料依類別目錄依字母順序排序時。


[![可排序 GridView s 類別目錄排序資料](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**圖 2**： 依分類排序資料的可排序 w s ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>步驟 2： 加入分隔符號資料列探索技術

與泛型、 可排序 GridView 完整，所有剩下的都只有能夠在每個唯一的排序群組之前 GridView 加入分隔符號資料列。 不過，如何這類資料列插入 GridView 嗎？ 基本上，我們需要來逐一查看的 GridView s 資料列，判斷其中的差異中各值之間的已排序的資料行，然後再加入適當的分隔符號資料列。 考慮這個問題，您似乎自然方案某處在於 GridView 的`RowDataBound`事件處理常式。 如我們所述[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-cs.md)教學課程，此事件處理常式通常使用於套用格式設定資料列層級根據 s 資料列的資料。 不過，`RowDataBound`事件處理常式不是解決方案，資料列無法從這個事件處理常式以程式設計方式新增至 GridView。 GridView 的`Rows`集合，其實是唯讀的。

若要將 GridView 中加入額外的資料列中，我們有三種選擇：

- 這些中繼資料分隔符號資料列加入繫結至 GridView 的實際資料
- 已繫結的 GridView 的資料之後，請加入更多`TableRow`GridView s 的執行個體控制集合
- 建立自訂的伺服器控制項，以延伸 GridView 控制項，並會覆寫這些方法負責建構 GridView 的結構

如果這項功能需要許多網頁上或跨多個網站，建立自訂的伺服器控制項可能會最好的方法。 不過，它會需要有相當多的程式碼和 GridView s 內部運作的完整複製瀏覽。 因此，我們將不考慮該選項在此教學課程。

其他兩種選項加入分隔符號資料列，實際資料的繫結至 GridView 和操作後的 s GridView 控制項集合其已繫結-攻擊問題以不同的方式，有些討論。

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>將資料列加入資料繫結至 GridView

在 GridView 繫結至資料來源，它會建立`GridViewRow`資料來源傳回的每一筆記錄。 因此，我們可以將所需的分隔符號資料列插入將分隔符號記錄新增至資料來源中，然後再繫結至 GridView。 圖 3 說明這個概念。


![其中一種技術，必須將分隔符號資料列加入至資料來源](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**圖 3**： 其中一種技術，必須將分隔符號資料列加入至資料來源


我將使用的詞彙分隔記錄以引號括住，因為沒有特殊的分隔記錄中;相反地，我們必須以某種方式的旗標做為分隔符號，而不是一般的資料列的資料來源中的特定資料錄。 針對我們的範例，我們重新繫結`ProductsDataTable`至 GridView，所組成的執行個體`ProductRows`。 我們可能會加上旗標的記錄做為分隔符號資料列設定其`CategoryID`屬性`-1`（因為這類的值，但是 t 通常存在）。

若要利用這項技術 d 我們需要執行下列步驟：

1. 以程式設計方式擷取的資料繫結至 GridView (`ProductsDataTable`執行個體)
2. 依據排序資料的 GridView s`SortExpression`和`SortDirection`屬性
3. 逐一查看`ProductsRows`中`ProductsDataTable`，尋找在排序資料行中的差異的地方，
4. 在每個群組的界限，插入分隔記錄`ProductsRow`執行個體至 DataTable，其中有它 s`CategoryID`設`-1`（或任何指定已決定將標示為分隔記錄的記錄）
5. 之後插入資料列分隔符號，以程式設計方式將資料繫結至 GridView

除了這五個步驟中，我們 d 也要提供給事件處理常式 GridView s`RowDataBound`事件。 在這裡，我們 d 檢查每一個`DataRow`並判斷它是否為分隔符號，第一列其`CategoryID`設定在當時`-1`。 如果是這樣，我們 d 可能會想要調整它的格式設定，或儲存格中所顯示的文字。

使用這項技術，將，以便排序群組界限需要比前面的一些工作，視需要同時也提供事件處理常式 GridView s`Sorting`事件並保留追蹤`SortExpression`和`SortDirection`值。

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>操作 GridView s 控制 s 其後集合已資料繫結

而不是訊息之前繫結至 GridView 的資料，我們可以將分隔符號資料列加入*之後*已繫結至 GridView 的資料。 在 GridView 的控制項階層架構，這實際上是建立資料繫結的程序只是`Table`執行個體所組成的資料列，其中每一個都儲存格的集合所組成的集合。 具體來說，GridView 的控制項集合包含`Table`物件，其根目錄`GridViewRow`(其衍生自`TableRow`類別) 中的每一筆記錄`DataSource`繫結至 GridView，和`TableCell`物件中每個`GridViewRow`中每個資料欄位的執行個體`DataSource`。

若要新增每個排序群組之間的分隔符號資料列，我們可以直接操作此控制項階層架構一旦建立之後。 我們可以放心在 GridView 的控制項階層架構已經建立的最後一次所呈現頁面的時間。 因此，這個方法會覆寫`Page`類別的`Render`方法，此時的 GridView s 最終控制項階層架構會更新以包含所需的分隔符號資料列。 圖 4 說明此程序。


[![替代技巧操作 GridView 的控制項階層架構](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**圖 4**： 替代技巧操作 GridView 的控制項階層架構 ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


此教學課程中，我們會使用這個第二種方法可自訂排序的使用者經驗。

> [!NOTE]
> 程式碼我 m 呈現在本教學課程根據提供的範例[Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s 部落格文章：[播放與 GridView 排序群組的位元](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx)。


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>步驟 3： 將分隔符號資料列加入至 GridView 的控制項階層架構

因為我們只想要將分隔符號資料列加入至 GridView 的控制項階層架構，已建立並在該頁面造訪最後一次建立其控制項階層架構之後，我們想要執行此新增結束頁面生命週期中，但實際的 GridView c 之前下時階層已呈現成 HTML。 此時我們可以完成這項作業的最新的可能點`Page`類別的`Render`事件，我們可以使用下列方法簽章我們的程式碼後置類別中覆寫：


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

當`Page`類別原始`Render`叫用方法`base.Render(writer)`每個頁面中的控制項呈現，產生根據其控制項階層架構的標記。 因此務必我們這兩個呼叫`base.Render(writer)`，以便在轉譯頁面時，控制項之前呼叫階層架構中，我們操作 GridView 的`base.Render(writer)`，如此分隔符號資料列已加入至之前的 GridView 的控制項階層架構s 已經呈現。

若要將排序群組標頭，我們需要確定使用者已要求資料進行排序。 根據預設，不會排序 GridView 的內容，並因此我們不 t 需要輸入排序的標頭的任何群組。

> [!NOTE]
> 如果您想要第一次載入頁面時，依特定的資料行來排序 GridView，呼叫 GridView 的`Sort`方法在第一個頁面造訪 （但不是會在後續回傳時）。 若要完成這項作業，加入此呼叫中的`Page_Load`內的事件處理常式`if (!Page.IsPostBack)`條件。 若要回頭參考[分頁和排序報表資料](paging-and-sorting-report-data-cs.md)教學課程的資訊，如需有關`Sort`方法。


假設資料經過排序下, 一步是要判斷哪一個資料行資料已排序，然後可以掃描尋找該資料行 s 中的差異資料列的值。 下列程式碼，以確保資料的已排序和尋找資料經過排序之資料行：


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

如果 GridView 有尚未為已排序，GridView 的`SortExpression`將尚未設定屬性。 因此，我們只想要加入分隔符號資料列，如果這個屬性有一些值。 若是如此，我們接下來必須決定的資料已排序的資料行的索引。 這會透過迴圈 GridView s`Columns`集合，其搜尋資料行`SortExpression`屬性等於 GridView 的`SortExpression`屬性。 除了資料行的索引中，我們也抓取`HeaderText`顯示分隔符號資料列時所用的屬性。

資料已排序的資料行的索引，最後一個步驟是列舉的 GridView 的資料列。 每個資料列中，我們需要判斷是否已排序資料行的值不同於先前的資料列 s 排序資料行的值。 因此，我們需要將新如果`GridViewRow`到在控制項階層架構的執行個體。 使用下列程式碼完成這個動作：


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

此程式碼會啟動以程式設計方式參考`Table`GridView 的控制項階層架構的根目錄中找到的物件，並建立名為的字串變數`lastValue`。 `lastValue`用來比較目前資料列 s 排序資料行值與上一個資料列的值。 下一步、 GridView s`Rows`會列舉集合，每個資料列已排序的資料行的值會儲存在`currentValue`變數。

> [!NOTE]
> 若要判斷特定資料列 s 排序資料行的值使用的儲存格的`Text`屬性。 這適用於 BoundFields，但將不適用 TemplateFields，CheckBoxFields，視等等。 我們會探討如何稍後說明替代 GridView 欄位。


`currentValue`和`lastValue`變數接著會比較。 如果它們之間的差異就必須在控制項階層架構中加入新的資料列分隔符號。 這會透過判斷索引`GridViewRow`中`Table`物件 s`Rows`集合中，建立新的`GridViewRow`和`TableCell`執行個體，然後再新增`TableCell`和`GridViewRow`至控制項階層架構。

注意分隔符號資料列 s 單獨`TableCell`會格式化，跨越整個寬度的 GridView 中使用格式化`SortHeaderRowStyle`CSS 類別，且其`Text`例如，它會顯示這兩個排序群組名稱 （例如類別） 的屬性和（例如飲料） 群組的值。 最後，`lastValue`更新的值`currentValue`。

用來格式化排序群組標頭資料列的 CSS 類別`SortHeaderRowStyle`在中指定`Styles.css`檔案。 可以自由使用任何樣式設定吸引您;我使用下列項目：


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

使用目前的程式碼中，排序介面時，加入排序群組標頭排序任何 BoundField （請參閱圖 5 顯示螢幕擷取畫面，供應商排序時）。 不過，當其他任何欄位類型 （例如 CheckBoxField 或 TemplateField） 來排序，排序群組標頭會找不到 （請參閱圖 6）。


[![排序介面由 BoundFields 排序時，包含排序群組標頭](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**圖 5**: 排序介面包含排序群組標頭時排序 BoundFields ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![排序群組標頭會遺失時排序 CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**圖 6**： 排序群組標頭會遺失時排序 CheckBoxField ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


由 CheckBoxField 排序時，會遺失排序群組標頭的原因是因為程式碼目前使用剛才`TableCell`s`Text`屬性來判斷每個資料列已排序的資料行的值。 CheckBoxFields，如`TableCell`s`Text`屬性是空的字串; 相反地，值是可透過位於中核取方塊 Web 控制項`TableCell`s`Controls`集合。

若要處理 BoundFields 以外的欄位類型，我們需要來增強程式碼位置`currentValue`變數指派給檢查中的核取方塊存在`TableCell`s`Controls`集合。 而不是使用`currentValue = gvr.Cells[sortColumnIndex].Text`，這段程式碼替換成下列：


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

此程式碼會檢查已排序的資料行`TableCell`來判斷是否有任何控制項中的目前資料列`Controls`集合。 如果有，而且第一個控制項的核取方塊，`currentValue`變數設為 是 或 否，根據核取方塊的`Checked`屬性。 否則，該值為擷取自`TableCell`s`Text`屬性。 這個邏輯可以複寫到處理任何 TemplateFields GridView 中可能存在的排序。

上述程式碼加入，排序群組標頭現在都已停止的 CheckBoxField 排序時 （請參閱圖 7）。


[![排序群組標頭會立即出現時排序 CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**圖 7**： 排序群組標頭會立即出現時排序 CheckBoxField ([按一下以檢視完整大小的影像](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> 如果您有產品`NULL`資料庫值`CategoryID`， `SupplierID`，或`UnitPrice`欄位，這些值會出現為 GridView 中的空白字串依預設，分隔符號 s 資料列文字表示的那些產品之`NULL`值將讀取像類別目錄: (也就是沒有 s 類別後的沒有名稱： 例如類別目錄： 飲料)。 如果您想要在此顯示值，您可以透過設定 BoundFields [ `NullDisplayText`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx)文字您想要顯示或指派時，您可以在轉譯方法中加入條件陳述式`currentValue`分隔符號資料列的`Text`屬性。


## <a name="summary"></a>總結

GridView 不包含許多內建的自訂排序的介面。 不過，在使用的位元的低階程式碼，它能夠調整 GridView 的控制項階層架構，以建立更多自訂的介面 s。 在此教學課程中我們可了解如何加入排序群組分隔符號資料列的可排序的 GridView，更輕鬆地識別不同的群組，以及這些群組的界限。 如需自訂排序介面的其他範例，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/) s [ASP.NET 2.0 GridView 排序的秘訣和訣竅](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx)部落格文章。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一頁](sorting-custom-paged-data-cs.md)
[下一頁](paging-and-sorting-report-data-vb.md)
