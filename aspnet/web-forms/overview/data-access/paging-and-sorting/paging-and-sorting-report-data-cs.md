---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: 分頁和排序報告資料 (C#) |Microsoft Docs
author: rick-anderson
description: 分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。 在本教學課程中我們將率先一睹加入排序和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 75a2206bf3db3af8859fe4de58f67135d31bba0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401918"
---
<a name="paging-and-sorting-report-data-c"></a>分頁和排序報告資料 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe)或[下載 PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> 分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。 在本教學課程中，我們將率先一睹加入排序和分頁加入我們的報告，我們將在未來的教學課程時建置。


## <a name="introduction"></a>簡介

分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。 比方說，搜尋時在線上書店的 ASP.NET 書籍，可能有數百個這類的書籍，但報告，列出搜尋結果會列出每頁只十個相符項目。 此外，結果可以依照標題、 價格、 頁數、 作者名稱等等。 雖然的過去 23 的教學課程檢驗了如何建置各種不同的報表，包括介面，允許新增、 編輯和刪除資料，我們不討論過如何排序資料，而且只發生分頁範例我們 ve 看到已與 DetailsView 和 FormView控制項。

在本教學課程中，我們會看到如何將排序和分頁加入我們的報告，可透過只會檢查幾個核取方塊。 不幸的是，這個簡單的實作有一些需要排序的介面，離開其缺點，分頁常式不專為有效率地分頁的大型結果集。 未來的教學課程將探討如何克服--現成的分頁和排序解決方案的限制。

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步驟 1： 加入分頁和排序教學課程的網頁

在開始本教學課程之前，可讓 s 先花點時間，將 ASP.NET 頁面，我們需要針對本教學課程和下列三個。 藉由建立新的資料夾中名為專案啟動`PagingAndSorting`。 接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![建立 PagingAndSorting 資料夾，並新增這些教學課程的 ASP.NET 網頁](paging-and-sorting-report-data-cs/_static/image1.png)

**圖 1**： 建立 PagingAndSorting 資料夾，並新增這些教學課程的 ASP.NET 網頁


接下來，開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。 此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並顯示這些教學課程中的項目符號清單中目前的區段。


![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**圖 2**： 將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx


若要有項目符號清單顯示分頁和排序我們將建立的教學課程，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並在編輯、 插入，以及刪除站台對應節點標記之後新增下列標記：


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-and-sorting-report-data-cs/_static/image3.png)

**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁


## <a name="step-2-displaying-product-information-in-a-gridview"></a>步驟 2: GridView 中顯示產品資訊

我們實際實作分頁和排序功能之前，讓第一次建立標準非-srotable，不可分頁的 GridView 會列出產品資訊。 這是工作我們完成之前多次在此教學課程系列讓這些步驟的 ve 應該很熟悉。 首先開啟`SimplePagingSorting.aspx`頁面上，並將 GridView 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性設`Products`。 接下來，建立新的 ObjectDataSource 使用 ProductsBLL 類別的`GetProducts()`方法傳回的所有產品資訊。


![擷取所有的產品使用 GetProducts() 方法的相關資訊](paging-and-sorting-report-data-cs/_static/image4.png)

**圖 4**： 擷取所有的產品使用 GetProducts() 方法的相關資訊


因為這是唯讀的報表時，沒有 s 不需要對應的 ObjectDataSource 秒`Insert()`， `Update()`，或`Delete()`方法，以對應`ProductsBLL`方法; 因此，（無） 從清單中選擇下拉式清單的 INSERT、 UPDATE和刪除索引標籤。


![選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤](paging-and-sorting-report-data-cs/_static/image5.png)

**圖 5**： 選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤


接下來，讓 s 自訂 GridView 的欄位，以便只有產品名稱、 供應商、 類別、 價格和已停止的狀態會顯示。 此外，隨時進行欄位層級格式變更時，例如調整`HeaderText`內容] 或 [格式化為貨幣的價格。 這些變更之後，請 GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

圖 6 顯示我們進行到目前為止透過瀏覽器檢視時。 請注意，頁面會列出所有在一個畫面中，顯示每個產品的名稱、 類別、 供應商、 價格、 產品已停止的狀態。


[![每個產品詳列](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**圖 6**： 每個產品詳列 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>步驟 3： 加入分頁支援

列出*所有*在某個畫面上的產品可能會導致使用者瀏覽資料的資訊負荷過重。 為了讓結果更容易管理，我們可以分割成較小的頁面資料的資料，並允許使用者在一頁資料透過一次。 若要完成，這只是檢查啟用分頁 核取方塊的 GridView s 智慧標籤 (這會將 GridView s [ `AllowPaging`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)至`true`)。


[![請檢查啟用分頁核取方塊，以新增分頁支援](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**圖 7**： 請檢查啟用分頁核取方塊以新增分頁支援 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image11.png))


啟用分頁限制每頁顯示的記錄數目，並將*分頁介面*至 GridView。 [圖 7] 所示的預設分頁介面是一連串的頁面數字，讓使用者能夠從一頁資料快速巡覽至另一個。 此分頁介面應該看起來很熟悉，因為我們已見識過它時在過去的教學課程中，新增 DetailsView 和 FormView 控制項的分頁支援。

在 DetailsView 和 FormView 控制項只會顯示每個頁面的單一記錄。 Gridview 裡，不過，會參考其[`PageSize`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx)來判斷每頁顯示的多少筆記錄 （此屬性預設為 10）。

您可以使用下列屬性自訂此 GridView、 DetailsView 和 FormView 的分頁介面：

- `PagerStyle` 表示分頁介面的樣式資訊可以指定設定，例如`BackColor`， `ForeColor`， `CssClass`， `HorizontalAlign`，依此類推。
- `PagerSettings` 包含一系列的可自訂的分頁介面功能的屬性`PageButtonCount`指出數值頁面 （預設值為 10） 的分頁介面中顯示的數字的最大數目，而[`Mode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx)表示分頁介面的運作方式，且可以設定為： 

    - `NextPrevious` 顯示下一個] 和 [上一步的按鈕，讓使用者能夠逐步向前或向後一頁一次
    - `NextPreviousFirstLast` 除了下一步 和 上一頁 按鈕，第一個和最後一個按鈕也會包含在內，可讓使用者快速移至第一個或最後一頁的資料
    - `Numeric` 顯示一系列可讓使用者立即跳到任何頁面的頁碼
    - `NumericFirstLast` 列印的頁碼，除了包含 第一個和最後一個按鈕，讓使用者能夠快速移至第一個或最後一頁的資料;第一個/最後一個按鈕才會顯示所有的數字的頁碼不能配合

此外，GridView、 DetailsView 和 FormView 所有優惠`PageIndex`和`PageCount`分別表示目前正在檢視的頁面和資料的總頁數的屬性。 `PageIndex`屬性索引從 0，也就是說，檢視資料的第一頁時開始`PageIndex`會等於 0。 `PageCount`相反地，啟動 計數 1，表示`PageIndex`僅限於值介於 0 和`PageCount - 1`。

可讓 s 花一點時間來改善我們的 GridView 的分頁介面的預設外觀。 具體來說，可讓 s 靠右對齊，淺灰色背景分頁介面。 而不是設定這些屬性直接透過 GridView s`PagerStyle`屬性，建立可讓 s 中的 CSS 類別`Styles.css`名為`PagerRowStyle`，然後指派`PagerStyle`s`CssClass`透過我們的佈景主題的屬性。 首先開啟`Styles.css`並新增下列 CSS 類別定義：


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

接下來，開啟`GridView.skin`檔案中`DataWebControls`資料夾內`App_Themes`資料夾。 如我們所述*主版頁面與網站導覽*教學課程，面板檔案可以用來指定網頁控制項的預設屬性值。 因此，增強現有的設定，以包含設定`PagerStyle`s`CssClass`屬性設`PagerRowStyle`。 此外，可讓 s 設定分頁介面，以顯示最多五個數值的頁面按鈕使用`NumericFirstLast`分頁介面。


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>分頁的使用者體驗

圖 8 顯示當 GridView s 啟用分頁 核取方塊已核取之後，透過瀏覽器瀏覽的網頁和`PagerStyle`並`PagerSettings`設定已透過`GridView.skin`檔案。 請注意如何唯一的十個記錄會顯示，且分頁介面表示我們正在檢視資料的第一頁。


[![使用已啟用分頁，一次顯示記錄的子集](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**[圖 8**： 只記錄的子集分頁已啟用] 會顯示一次 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image14.png))


當使用者按一下其中一個頁面中的數字的分頁介面上時，回傳是兩邊彼此乾瞪眼，頁面會重新載入要求的頁面的記錄的顯示。 選擇要檢視資料的最後一頁之後，[圖 9] 顯示結果。 請注意，最後一頁只會有一筆記錄;這是因為有 81 記錄總數，導致 含有單一記錄在每個頁面，再加上一頁的 10 筆記錄的八個頁面。


[![按一下頁碼造成回傳，並顯示適當的記錄的子集](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**圖 9**： 按一下頁碼造成回傳，並顯示適當的資料錄子集 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>工作流程-s 伺服器端分頁

當使用者按一下分頁介面中的按鈕時，回傳是兩邊彼此乾瞪眼和下列伺服器端工作流程開始：

1. GridView s （或 DetailsView 或 FormView）`PageIndexChanging`引發事件
2. ObjectDataSource 重新要求*所有*BLL; 中的資料的 GridView s`PageIndex`和`PageSize`屬性值來判斷需要在 GridView 中顯示的 BLL 從傳回的記錄
3. GridView 的`PageIndexChanged`引發事件

在步驟 2，ObjectDataSource 重新要求所有的資料來源的資料。 這種樣式的分頁通常稱為*預設分頁*，s 的分頁行為的預設設定時，所使用如同`AllowPaging`屬性設`true`。 預設值這個分頁資料 Web 控制項擷取所有記錄每一頁的資料，即使只記錄的子集實際會轉譯成 HTML s 傳送到瀏覽器。 除非由 BLL 或 ObjectDataSource 快取的資料庫資料，預設的分頁是夠大的結果集或與許多並行使用者的 web 應用程式無法運作。

在下一個教學課程中，我們將檢驗如何實作*自訂分頁*。 使用自訂分頁時，您可以特別指示，只擷取的記錄所需的要求的頁面資料的一組精確 ObjectDataSource。 您可以想像得到，自訂分頁便可大幅提升對大型結果集進行分頁的效率。

> [!NOTE]
> 雖然有許多同時使用者透過夠大的結果集或網站翻頁時，預設的分頁功能不適合，了解自訂分頁需要更多的變更和精力來實作，並不簡單，只要選取核取方塊，（因為是預設值分頁）。 因此，預設的分頁功能可能是小型、 低流量網站或相當小的結果進行分頁的設定時，因為它的理想選擇 s 更容易、 更快速地實作。


比方說，如果我們知道，我們會在資料庫中，永遠不會有 100 個以上的產品，藉由自訂分頁的最小的效能改善的實作所需的心力可能位移。 如果，不過，我們可能一天有數千或數以萬計的產品*不*實作自訂的分頁會大幅會拖累我們的應用程式的延展性。

## <a name="step-4-customizing-the-paging-experience"></a>步驟 4： 自訂分頁體驗

資料 Web 控制項提供許多可用來增強使用者的分頁經驗的屬性。 `PageCount`屬性，例如，指出有多少總頁數，雖然`PageIndex`屬性表示目前所造訪的頁面，並可以快速移至特定頁面的 使用者設定。 若要說明如何使用這些屬性來改善使用者的分頁體驗，可讓新增標籤的 s Web 控制對我們會通知使用者頁面的頁面它們 re 目前瀏覽時，以及讓他們快速跳至指定的頁面 DropDownList 控制項.

首先，將 Label Web 控制項新增至您的頁面，設定其`ID`屬性，以`PagingInformation`，並清除其`Text`屬性。 接下來，建立事件處理常式 GridView s`DataBound`事件，並新增下列程式碼：


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

這個事件處理常式指派`PagingInformation`標籤 s`Text`訊息，通知使用者頁面目前瀏覽屬性`Products.PageIndex + 1`多少的總頁數超出`Products.PageCount`(我們將新增 1 至`Products.PageIndex`屬性因為`PageIndex`編製索引從 0 開始)。 我選擇指派此標籤 s`Text`中的屬性`DataBound`事件處理常式，而不是`PageIndexChanged`事件處理常式因為`DataBound`事件引發時，每次資料繫結至 GridView 而`PageIndexChanged`只有事件處理常式頁面索引變更時引發。 當 GridView 一開始會繫結的第一頁上的資料瀏覽`PageIndexChanging`事件不 t 引發 (而`DataBound`事件沒有)。

此步驟中，使用者現在會顯示一則訊息指出他們造訪哪些頁面而的資料有多少的總頁數。


[![會顯示目前的頁碼和總頁數](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**圖 10**： 會顯示目前的頁碼和總頁數 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image20.png))


除了標籤控制項，可讓 s 也新增 DropDownList 控制項，與目前所檢視的頁面，選取列出的 GridView 內的頁碼。 這意思是，使用者可以快速跳從目前的頁面之間只需從 DropDownList 中選取新的頁面索引。 開始將 dropdownlist 進行加入至設計工具中，設定其`ID`屬性設`PageList`並檢查它的智慧標籤的 啟用 AutoPostBack 選項。

接下來，回到`DataBound`事件處理常式，並新增下列程式碼：


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

此程式碼一開始會清除中的項目`PageList`DropDownList。 這似乎是多餘的因為一個不 t 預期的頁數，若要變更，但其他使用者可能會同時使用系統、 新增或移除記錄從`Products`資料表。 這類插入或刪除無法改變資料的頁數。

接下來，我們需要建立一次列印的頁碼已對應至目前的 GridView`PageIndex`預設選取。 我們完成這項作業具有從 0 到迴圈`PageCount - 1`，加入新`ListItem`中每個反覆項目和設定其`Selected`屬性設定為 true，如果目前的反覆項目索引等於 GridView 的`PageIndex`屬性。

最後，我們需要建立事件處理常式 DropDownList s`SelectedIndexChanged`引發的事件，每當使用者選擇不同的項目，從清單中。 若要建立這個事件處理常式，只要按兩下 DropDownList 在設計師中，然後新增下列程式碼：


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

如 [圖 11] 所示，只要變更的 GridView s`PageIndex`屬性會導致資料重新繫結至 GridView。 在 GridView`DataBound`事件處理常式中，適當的 DropDownList`ListItem`已選取。


[![使用者會自動前往第六個頁面時選取的頁面 6 下拉式清單項目](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**圖 11**: 使用者會自動前往第六個頁面時選取的頁面 6 下拉式清單項目 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>步驟 5： 新增雙向排序支援

加入雙向排序支援很簡單，只要新增分頁支援只需要檢查 GridView s 智慧標籤的 啟用排序選項 (可設定 GridView s [ `AllowSorting`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)至`true`)。 顯示如下的 GridView 的欄位標頭的每個 Linkbutton，按下時，造成回傳並傳回根據按下的資料行以遞增順序排序的資料。 再次按一下相同的標頭 LinkButton 重新排序的資料，以遞減順序。

> [!NOTE]
> 如果您使用自訂的資料存取層，而不是具類型資料集，您不能啟用排序選項中的 GridView s 智慧標籤。 繫結至原生支援 排序的資料來源的 Gridview 有可以使用此核取方塊。 輸入資料集提供的立即可用的排序支援，因為提供 ADO.NET DataTable`Sort`方法，叫用時，排序 s 使用指定之準則的 Datarow 的 DataTable。


如果您的 DAL 未傳回 dal 的原生支援可讓您排序您要設定將排序的資訊傳遞至商業邏輯層，可以排序資料或有資料 ObjectDataSource 排序的物件。 我們將探討如何在未來的教學課程中排序資料，商務邏輯和資料存取層級。

排序的 Linkbutton 會轉譯為 HTML 超連結，其目前的色彩 （藍色的未瀏覽的連結，然後瀏覽連結為深紅色） 衝突與標頭資料列的背景色彩。 相反地，可讓 s 具有白色的不論是否在顯示的所有標頭資料列連結它們 ve 已瀏覽與否。 這可藉由將下列內容加入`Styles.css`類別：


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

此語法表示要顯示這些項目使用 HeaderStyle 類別內的超連結時，使用白色文字。

在此 CSS 新增功能之後, 造訪透過瀏覽器頁面時您的畫面看起來應該類似 圖 12。 價格欄位標頭連結已按下之後，特別是，圖 12 顯示結果。


[![以遞增順序 UnitPrice 已經排序結果](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**圖 12**: 結果已經排序以遞增順序 UnitPrice ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>檢查 排序的工作流程

所有的 GridView 欄位 BoundField CheckBoxField、 TemplateField，還有等`SortExpression`屬性，指出應該用來排序標頭連結該欄位 s 按一下時排序資料的運算式。 也有 GridView`SortExpression`屬性。 排序的標頭，在按一下 LinkButton，GridView 會指派該欄位 s`SortExpression`值以其`SortExpression`屬性。 接下來，資料是從 ObjectDataSource 重新擷取，而且排序 GridView s 根據`SortExpression`屬性。 下列清單詳細說明瓿當終端使用者排序 GridView 中的資料的步驟順序：

1. GridView s [Sorting 事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)引發
2. GridView s [ `SortExpression`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)設定為`SortExpression`欄位的 LinkButton 已按下其排序的標頭
3. ObjectDataSource 重新擷取所有的 BLL 中的資料，然後排序 GridView s 上使用的資料 `SortExpression`
4. GridView 的`PageIndex`屬性重設為 0，表示排序使用者時傳回給資料 （假設已實作的分頁支援） 的第一頁
5. GridView s [ `Sorted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)引發

使用預設分頁時，預設的排序選項重新擷取像是*所有*記錄的 BLL。 使用未分頁排序時，或使用排序時的預設分頁，該處 s 沒有辦法避免這樣叫用 （除了快取的資料庫資料） 的效能。 不過，我們會看到在未來的教學課程中，它可以使用自訂分頁時，有效率地排序的資料。

每個 GridView 欄位繫結時 ObjectDataSource 至 GridView 透過下拉式清單中的 GridView s 智慧標籤，自動擁有其`SortExpression`屬性中的資料欄位的名稱指派`ProductsRow`類別。 例如， `ProductName` BoundField s`SortExpression`設定為`ProductName`，如下列宣告式標記中所示：


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

您可以設定欄位，讓它 s 不可排序清除其`SortExpression`（將它指派給空字串） 的屬性。 若要舉例來說，想像一下，我們不想要讓我們依價格排序產品的客戶。 `UnitPrice` BoundField 的`SortExpression`從宣告式標記，或是透過 [欄位] 對話方塊中 （也就是可存取上的 GridView s 智慧標籤中的 [編輯資料行] 連結即可），就可以移除屬性。


![以遞增順序 UnitPrice 已經排序結果](paging-and-sorting-report-data-cs/_static/image27.png)

**圖 13**： 以遞增順序 UnitPrice 已經排序結果


一次`SortExpression`屬性已移除`UnitPrice`BoundField 頁首會轉譯為文字而不是連結，藉此防止使用者排序資料的價格。


[![藉由移除 SortExpression 屬性，使用者可以不會再排序依價格的產品](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**圖 14**： 藉由移除 SortExpression 屬性，使用者可以不會再排序產品的價格 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>以程式設計的方式排序 GridView

您也可以排序 GridView 的內容以程式設計方式使用 GridView s [ `Sort`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)。 只需傳入`SortExpression`連同排序所依據的值[ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`或`Descending`)，而且會重新排序 GridView 的資料。

想像一下，我們關閉排序原因`UnitPrice`是因為我們所擔心我們的客戶會只是購買只最低價的產品。 不過，我們想要鼓勵他們購買成本最高的產品，因此我們 d 希望能夠排序到最少的產品價格，但只會從最耗費資源的價格。

若要完成這 Button Web 控制項加入頁面上，設定其`ID`屬性，以`SortPriceDescending`，並將其`Text`價格依排序的屬性。 接下來，建立 s 按鈕的 事件處理常式`Click`按兩下按鈕控制項設計工具中的事件。 這個事件處理常式中加入下列程式碼：


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

按一下此按鈕會傳回使用者第一頁從耗費最多的資源成本最低 （請參閱 圖 15） 的價格，依排序的產品。


[![按一下按鈕排序的產品成本最高到最低](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**圖 15**： 按一下按鈕排序產品從成本最高到最低 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>總結

在本教學課程，我們了解如何實作分頁和排序功能的預設值，這兩者都是簡單，只要選取核取方塊項目 ！ 當使用者排序，或透過資料頁時，事件發生類似的工作流程：

1. 回傳是兩邊彼此乾瞪眼
2. 資料 Web 控制項 s 預先層級事件引發 (`PageIndexChanging`或`Sorting`)
3. 所有的資料重新擷取 ObjectDataSource
4. 後置資料 Web 控制項 s 層級的事件引發 (`PageIndexChanged`或`Sorted`)

實作基本的分頁和排序是變得輕而易舉，利用更有效率的自訂分頁，或進一步加強的分頁或排序介面必須施加投入更多心力。 未來的教學課程將探討這些主題。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [下一步](efficiently-paging-through-large-amounts-of-data-cs.md)
