---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: "分頁和排序報表資料 (VB) |Microsoft 文件"
author: rick-anderson
description: "分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。 在此教學課程中我們將加入排序第一眼和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ef1bb0b68a46535e3320834a0374b9a4f66182c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="paging-and-sorting-report-data-vb"></a>分頁和排序報表資料 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe)或[下載 PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> 分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。 在本教學課程中，我們要花第一眼加入排序和分頁我們的報表，我們將在未來教學課程時建置。


## <a name="introduction"></a>簡介

分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。 比方說，搜尋時在線上書店 ASP.NET 書籍，可能有數百個這類的書籍，但列出的搜尋結果此報表會列出每個頁面僅十個相符項目。 此外，可以根據標題、 價格、 頁面計數、 作者姓名等排序結果。 雖然的過去 23 教學課程檢查如何建立各種不同的報表，包括介面，允許以新增、 編輯和刪除資料，我們不如何排序資料，而且只在查看過分頁範例我們我們看到已 FormView DetailsView 與控制項。

在本教學課程中，我們會看到如何加入排序和分頁報表，這可以藉由只檢查幾個核取方塊來達成。 不幸的是，這個簡單的實作就有其缺點排序介面會保留到需要位元和分頁常式不專為有效率地分頁的大型結果集。 未來的教學課程將探討如何克服--現成的分頁和排序解決方案的限制。

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步驟 1： 加入分頁和排序教學課程的 Web 網頁

開始本教學課程之前，可讓 s 先花點時間加入我們需要針對此教學課程中，下一步的三個 ASP.NET 網頁。 藉由建立新的資料夾中名為專案啟動`PagingAndSorting`。 接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![建立 PagingAndSorting 資料夾，以及新增的教學課程的 ASP.NET 頁面](paging-and-sorting-report-data-vb/_static/image1.png)

**圖 1**： 建立 PagingAndSorting 資料夾，以及新增的教學課程的 ASP.NET 頁面


接下來，開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。 此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，列舉站台對應，並在項目符號清單中目前區段中顯示這些教學課程。


![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**圖 2**： 將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx


若要顯示分頁和排序教學課程，我們將建立的項目符號清單後，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並編輯、 插入，以及刪除站台對應節點標記後面加入下列標記：


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-and-sorting-report-data-vb/_static/image3.png)

**圖 3**： 更新包含新的 ASP.NET 網頁站台對應


## <a name="step-2-displaying-product-information-in-a-gridview"></a>步驟 2： 在 GridView 中顯示產品資訊

我們實際上會實作分頁和排序功能之前，可讓第一次建立標準非-srotable，不可分頁的 GridView 會列出產品資訊。 這是工作我們 ve 多次之前在此教學課程系列因此這些步驟的完成應該很熟悉。 先開啟`SimplePagingSorting.aspx`頁面上，並將 GridView 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性`Products`。 接下來，建立新的 ObjectDataSource 使用 ProductsBLL 類別的`GetProducts()`方法來傳回所有產品資訊。


![擷取的所有產品使用 GetProducts() 方法的相關資訊](paging-and-sorting-report-data-vb/_static/image4.png)

**圖 4**： 擷取的所有產品使用 GetProducts() 方法的相關資訊


由於此報告可是唯讀的報表時，有 s 不需要對應 ObjectDataSource s `Insert()`， `Update()`，或`Delete()`方法來對應`ProductsBLL`方法; 因此，（無） 從清單中選擇下拉式清單的 INSERT、 UPDATE和刪除索引標籤。


![選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤](paging-and-sorting-report-data-vb/_static/image5.png)

**圖 5**： 選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤


接下來，讓自訂的 GridView 的欄位以便只產品名稱、 供應商、 分類、 價格和已停止的狀態會顯示 s。 此外，可自由進行欄位層級格式變更時，例如調整`HeaderText`屬性或格式化為貨幣的價格。 這些變更之後，請 GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

圖 6 為止顯示進度，透過瀏覽器檢視時。 請注意頁面列出所有在一個畫面中，顯示每個產品的名稱、 類別、 供應商、 價格、 產品，並已停止的狀態。


[![列出每個產品](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**圖 6**： 列出每個產品 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>步驟 3： 加入分頁支援

列出*所有*的上一個螢幕的產品可能會導致資訊的使用者瀏覽資料的多載。 為了讓結果更容易管理，我們可以分解成較小的頁面資料的資料，並允許使用者在一頁資料透過一次。 若要完成這只會檢查啟用分頁 核取方塊的 GridView s 智慧標籤 (這會設定 GridView s [ `AllowPaging`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)至`true`)。


[![選取要加入的分頁支援啟用分頁核取方塊](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**圖 7**： 檢查啟用分頁核取方塊以新增分頁支援 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image11.png))


啟用分頁限制每頁顯示的記錄數目，並將*分頁介面*至 GridView。 圖 7 所示的預設分頁介面是一連串的頁面數字，讓使用者快速瀏覽一頁的資料從另一個。 此分頁介面應該看起來很熟悉，當我們 ve DetailsView 和 FormView 控制項加入分頁支援，在過去的教學課程時看到它。

DetailsView 和 FormView 控制項只會顯示每個頁面的單一記錄。 在 GridView，不過，參照其[`PageSize`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.pagesize.aspx)來判斷每頁顯示的多少筆記錄 （此屬性預設為 10）。

使用下列屬性可以自訂此 GridView、 DetailsView 和 FormView 的分頁介面：

- `PagerStyle`表示樣式資訊的分頁介面。可以指定設定，例如`BackColor`， `ForeColor`， `CssClass`， `HorizontalAlign`，依此類推。
- `PagerSettings`包含屬性，可以自訂分頁介面; 功能的 first`PageButtonCount`表示數值頁面 （預設值為 10） 的分頁介面中顯示的數字的最大數目; [ `Mode`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.pagersettings.mode.aspx)表示分頁介面如何運作，且可以設定為： 

    - `NextPrevious`顯示下一個] 和 [上一步的按鈕，讓使用者逐步向前或向後一頁
    - `NextPreviousFirstLast`除了下一步 和 上一步 按鈕，第一個和最後一個 按鈕也會包含，讓使用者快速移至第一個或最後一頁的資料
    - `Numeric`顯示一系列的頁碼，讓使用者立即跳至任何頁面
    - `NumericFirstLast`列印的頁碼，除了包含 第一個和最後一個按鈕，讓使用者快速移至第一個或最後一頁的資料;第一個/最後一個按鈕只會顯示於所有數字的頁碼不能配合

此外，GridView、 DetailsView 和所有提供的 FormView`PageIndex`和`PageCount`屬性，分別表示目前正在檢視的頁面和資料的總頁數。 `PageIndex`屬性具有索引 0，表示檢視資料的第一頁時開始`PageIndex`會等於 0。 `PageCount`相反地，啟動 計數 1，表示`PageIndex`僅限於的值介於 0 和`PageCount - 1`。

可讓 s 花一點時間來改善我們的 GridView 的分頁介面的預設外觀。 具體來說，可讓 s 擁有分頁介面靠右對齊，淺灰色背景。 而不是設定這些屬性直接透過 GridView s`PagerStyle`屬性，建立讓 s 中的 CSS 類別`Styles.css`名為`PagerRowStyle`然後指派`PagerStyle`s`CssClass`透過我們的佈景主題的屬性。 先開啟`Styles.css`並新增下列 CSS 類別定義：


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

接下來，開啟`GridView.skin`檔案`DataWebControls`資料夾內`App_Themes`資料夾。 如我們所述*主版頁面和站台瀏覽*教學課程，面板檔案可以用來指定 Web 控制項的預設屬性值。 因此，增強現有的設定，以包含設定`PagerStyle`s`CssClass`屬性`PagerRowStyle`。 此外，讓 s 設定分頁介面，以顯示最多五個數值頁面按鈕使用`NumericFirstLast`分頁介面。


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>分頁的使用者經驗

圖 8 顯示網頁時已檢查的 GridView s 啟用分頁核取方塊之後，透過瀏覽器瀏覽和`PagerStyle`和`PagerSettings`組態可能已透過`GridView.skin`檔案。 請注意如何只有 10 記錄會顯示，且分頁介面指出我們正在檢視資料的第一頁。


[![以啟用分頁，一次顯示記錄的子集](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**圖 8**： 記錄的子集分頁已啟用 會顯示一次 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image14.png))


當使用者按一下其中一個分頁介面中的頁碼時，回傳展示和頁面會重新載入要求頁面的記錄的顯示。 圖 9 顯示選擇以檢視資料的最後一頁之後的結果。 請注意，最後一頁只會有一筆記錄。這是因為有 81 記錄，導致八個頁面，以單獨的記錄在每個頁面，再加上一頁的 10 筆記錄的總計。


[![按一下頁碼導致回傳，並顯示記錄的適當子集合](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**圖 9**： 按一下頁碼導致回傳，並顯示適當的資料錄子集 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>分頁的伺服器端工作流程

當使用者按一下分頁介面中的按鈕時，回傳展示和下列伺服器端工作流程會開始：

1. GridView s （或 DetailsView 或 FormView）`PageIndexChanging`事件引發
2. ObjectDataSource 重新要求*所有*BLL; 資料的 GridView s`PageIndex`和`PageSize`屬性值可用來判斷哪些記錄從 BLL 傳回需要在 GridView 中顯示
3. GridView 的`PageIndexChanged`事件引發

步驟 2 中，在 ObjectDataSource 重新要求所有其資料來源的資料。 這種樣式的分頁通常稱為*預設分頁*，因為它 s 分頁行為時預設使用的設定`AllowPaging`屬性`true`。 預設值這個分頁 Web 控制項的資料擷取所有記錄每一頁的資料，即使記錄的子集實際會轉譯成 HTML s 傳送到瀏覽器。 資料庫資料會由 BLL 或 ObjectDataSource 快取，除非預設分頁是夠大的結果集或具有許多並行使用者的 web 應用程式處於無法運作。

在下一個教學課程中，我們將檢驗如何實作*自訂分頁*。 使用自訂分頁時，您可以特別指示 ObjectDataSource 來只擷取精確的記錄所需的資料要求的頁面集合。 您可以想像，自訂分頁可大幅提升效率的大型結果集進行分頁。

> [!NOTE]
> 雖然有許多同時使用者分頁夠大的結果集，或站台時，就不適當預設分頁，了解自訂分頁需要更多的變更和投入時間來實作，而且不只要選取核取方塊，（因為是預設值分頁）。 因此，預設分頁可能小型、 低流量網站或相對較小的結果進行分頁的設定時，因為它的理想選擇 s 更輕鬆且快速實作。


例如，如果我們知道我們永遠不會有超過 100 個產品在資料庫中，自訂分頁所用的最少的效能改善由實作所需的工作可能位移。 不過，我們可能一天有數千、 數萬數千張的產品，如果*不*實作自訂分頁會大幅阻礙我們的應用程式的延展性。

## <a name="step-4-customizing-the-paging-experience"></a>步驟 4： 自訂分頁體驗

Web 控制項的資料提供許多可用來加強使用者的分頁體驗的屬性。 `PageCount`屬性，例如，指出有多少總頁數，雖然`PageIndex`屬性指出目前正在瀏覽的網頁，而且可以快速移動至特定頁面的 使用者設定。 若要說明如何使用這些屬性來改善使用者的分頁體驗，讓 s 將標籤加入 Web 控制項加入我們會通知使用者哪一頁的頁面它們 re 目前造訪以及 DropDownList 控制項使其能快速跳至指定的頁面.

首先，將標籤 Web 控制項加入至您的頁面，設定其`ID`屬性`PagingInformation`，並以清除其`Text`屬性。 接下來，建立事件處理常式 GridView s`DataBound`事件並加入下列程式碼：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

這個事件處理常式指派`PagingInformation`標籤 s`Text`屬性至訊息，通知使用者頁面目前瀏覽`Products.PageIndex + 1`多少的總頁數超出`Products.PageCount`(我們將加入 1 到`Products.PageIndex`屬性因為`PageIndex`具有索引 0 開始)。 選取指派此標籤 s`Text`屬性中的`DataBound`與事件處理常式`PageIndexChanged`事件處理常式因為`DataBound`就會引發事件每次資料繫結至 GridView 而`PageIndexChanged`事件處理常式只頁面索引變更時引發。 當 GridView 一開始是資料繫結的第一頁上造訪`PageIndexChanging`事件規定 t 引發 (而`DataBound`事件沒有)。

與此新增功能，使用者現在會顯示訊息，指出他們造訪哪些頁面與有的資料有多少總頁數。


[![會顯示目前的頁碼和總頁數](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**圖 10**： 會顯示目前的頁碼和總頁數 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image20.png))


除了 Label 控制項，可讓 s 也加入 DropDownList 控制項，其中列出與目前檢視的頁面，選取在 GridView 中的頁碼。 這裡的概念是，使用者可以快速跳從目前網頁到另一個，只要從 DropDownList 選取新的頁面索引。 啟動者 DropDownList 加入設計工具中，設定其`ID`屬性`PageList`並檢查其智慧標籤的 啟用 AutoPostBack 選項。

接下來，返回`DataBound`事件處理常式並加入下列程式碼：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

此程式碼一開始會清除中的項目`PageList`DropDownList。 這似乎是多餘的因為一個不 t 預期的頁數，若要變更，但其他使用者可能會同時使用系統、 新增或移除記錄`Products`資料表。 這類插入或刪除無法改變資料的頁數。

接下來，我們需要重新建立頁碼有一個對應至目前的 GridView`PageIndex`依預設已選取。 我們完成這項作業以及從 0 到迴圈`PageCount - 1`，加入新`ListItem`在每個反覆項目和設定其`Selected`屬性設定為 true，如果目前的反覆項目索引等於 GridView 的`PageIndex`屬性。

最後，我們需要建立事件處理常式 DropDownList s`SelectedIndexChanged`引發的事件，每當使用者選取不同的項目從清單中。 若要建立此事件處理常式，只需按兩下 DropDownList 在設計師中，然後加入下列程式碼：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

圖 11 顯示，因為只要變更 GridView 的`PageIndex`屬性會導致重新繫結至 GridView 的資料。 在 GridView s`DataBound`事件處理常式，適當的 DropDownList`ListItem`已選取。


[![使用者會自動前往第六個頁面時選取 頁面 6 下拉式清單項目](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**圖 11**: 使用者會自動前往第六個頁面時選取 頁面 6 下拉式清單項目 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>步驟 5： 加入雙向排序支援

加入雙向排序支援很簡單，只新增分頁支援只會檢查 GridView s 智慧標籤的 啟用排序選項 (它會設定 GridView s [ `AllowSorting`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)至`true`)。 顯示如下的 GridView 的欄位標頭的每個 LinkButtons，按下時、 導致回傳，將資料依按資料行，以遞增順序排序。 再次按一下相同的標頭 LinkButton 重新排序的資料，以遞減的順序。

> [!NOTE]
> 如果您使用自訂的資料存取層，而不是型別資料集，您可能沒有啟用排序選項的 GridView s 智慧標籤。 繫結至原生支援排序的資料來源的 GridViews 有使用此核取方塊。 型別資料集提供的方塊外排序支援，因為 ADO.NET DataTable 提供`Sort`方法，叫用時，排序資料表 s Datarow 使用指定的準則。


如果您的 DAL dal 不會傳回原生支援排序您必須設定排序資訊傳遞到商務邏輯層，可以排序資料，或具有資料 ObjectDataSource 排序的物件。 我們將探討如何排序資料，在商務邏輯和資料存取層在未來的教學課程。

排序 LinkButtons 會轉譯為 HTML 的超連結，其目前的色彩 （未瀏覽的連結和瀏覽連結為深紅色藍色） 相衝突的標頭資料列的背景色彩。 O d e s 相反地，具有所有標頭資料列連結以白色，無論它們 ve 已瀏覽或不。 這可以透過加入下列命令以`Styles.css`類別：


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

這個語法表示要顯示這些項目使用 HeaderStyle 類別內的超連結時，使用白色文字。

之後此 CSS 新增功能，當瀏覽透過瀏覽器頁面螢幕看起來應該類似於圖 12。 已按下價格欄位 s 標頭連結之後，特別是，圖 12 顯示結果。


[![以遞增順序單價已經排序結果](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**圖 12**: 結果有已依照依遞增順序 UnitPrice ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>檢查 排序的工作流程

所有的 GridView 欄位 BoundField CheckBoxField、 TemplateField，而且等有`SortExpression`屬性，指出應該用來排序資料，在按下欄位 s 排序標頭連結時的運算式。 在 GridView 還有`SortExpression`屬性。 在 GridView 時排序的標頭，LinkButton 已按下，指派欄位 s`SortExpression`值設定為其`SortExpression`屬性。 接下來，資料是從 ObjectDataSource 重新擷取，而且排序 GridView s 根據`SortExpression`屬性。 下列清單詳細說明步驟的順序，顯示瓿當使用者排序 GridView 中的資料：

1. GridView s [Sorting 事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)引發
2. GridView s [ `SortExpression`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)設`SortExpression`欄位的 LinkButton 已按下其排序的標頭
3. ObjectDataSource 重新擷取的所有資料從 BLL，，然後排序的資料，使用 GridView s`SortExpression`
4. GridView 的`PageIndex`屬性重設為 0，表示排序使用者時傳回第一頁的資料 （假設已實作的分頁支援）
5. GridView s [ `Sorted`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)引發

使用預設分頁時，預設的排序選項重新擷取像*所有*記錄 BLL 的。 使用分頁沒有排序時或在使用含有排序預設分頁時，那里 s 沒有方法可以避免這個效能影響 （除非快取的資料庫資料）。 不過，我們會看到在未來的教學課程中，如它 s 能夠有效率地使用自訂分頁時，排序資料。

每個 GridView 欄位時 ObjectDataSource 繫結至 GridView 透過下拉式清單中的清單 GridView s 智慧標籤會自動擁有其`SortExpression`指派中的資料欄位的名稱屬性`ProductsRow`類別。 例如， `ProductName` BoundField s`SortExpression`設`ProductName`，如下列宣告式標記中所示：


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

您可以設定欄位，讓它 s 不可清除排序其`SortExpression`（將其指派給空字串） 的屬性。 為了說明這點，假設我們 professionals t 想要讓客戶排序我們的產品價格。 `UnitPrice` BoundField 的`SortExpression`從宣告式標記，或透過 欄位 對話方塊 （也就是可存取，即可編輯資料行中的連結 GridView s 智慧標籤），就可以移除屬性。


![以遞增順序單價已經排序結果](paging-and-sorting-report-data-vb/_static/image27.png)

**圖 13**： 已經單價以遞增順序排序結果


一次`SortExpression`已移除屬性，以`UnitPrice`BoundField，頁首會轉譯為文字而不是連結，藉此防止使用者排序資料的價格。


[![藉由移除 SortExpression 屬性，使用者不再可以排序產品的價格](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**圖 14**： 藉由移除 SortExpression 屬性，使用者不再可以排序產品的價格 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>以程式設計的方式排序 GridView

您也可以排序 GridView 內容以程式設計方式使用 GridView s [ `Sort`方法](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sort.aspx)。 只傳入`SortExpression`排序所依據連同值[ `SortDirection` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`或`Descending`)，而且會重新排序 GridView 的資料。

想像一下，我們關閉排序原因`UnitPrice`因為我們擔心我們的客戶會只是購買只低價產品。 不過，我們想要建議他們購買成本最高的產品，因此，我們 d 希望他們能夠排序到最少的產品價格，但只能從成本最高價格。

若要完成此按鈕 Web 控制項加入頁面上，設定其`ID`屬性`SortPriceDescending`，及其`Text`價格依排序的屬性。 接下來，建立事件處理常式按鈕 s`Click`按兩下按鈕控制項設計工具中的事件。 這個事件處理常式中加入下列程式碼：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

按一下此按鈕傳回使用者第一頁的價格，從最便宜 （請參閱圖 15） 成本最高依排序產品。


[![按一下按鈕訂單的產品成本最高到最低](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**圖 15**： 按一下按鈕的訂單產品從成本最高到最低 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>總結

在本教學課程，我們可了解如何實作分頁和排序功能的預設值，這兩者都是簡單，只要選取核取方塊 ！ 當使用者排序或資料頁時，可以掀起類似的工作流程：

1. 回傳展示
2. 資料 Web 控制項 s 預先層級事件引發 (`PageIndexChanging`或`Sorting`)
3. ObjectDataSource 重新擷取的所有資料
4. 後置資料 Web 控制項 s 層級的事件引發 (`PageIndexChanged`或`Sorted`)

雖然實作基本的分頁和排序更是輕而易舉，利用更有效率的自訂分頁，或進一步加強分頁或排序介面必須諸較多工作。 未來的教學課程將會探索這些主題。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一頁](creating-a-customized-sorting-user-interface-cs.md)
[下一頁](efficiently-paging-through-large-amounts-of-data-vb.md)
