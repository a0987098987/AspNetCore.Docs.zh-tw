---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: 顯示資料，以在 DataList 和中繼器控制項中 (C#) |Microsoft 文件
author: rick-anderson
description: 在先前的教學課程中，我們已使用 GridView 控制項來顯示資料。 從開始本教學課程中，我們會審視建置與一般報表模式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: a329ff5d598156e613e3b5ef370d9d1147e4ea61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>顯示資料，以在 DataList 和中繼器控制項中 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe)或[下載 PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> 在先前的教學課程中，我們已使用 GridView 控制項來顯示資料。 從開始本教學課程中，我們會審視建置通用的報告模式使用 DataList 和中繼器控制項中，從開始利用這些控制項顯示資料的基本概念。


## <a name="introduction"></a>簡介

中的所有範例整個過去 28 教學課程中，如果我們需要顯示我們的 GridView 控制項開啟資料來源的多筆記錄。 GridView 會轉譯為資料來源中的每一筆記錄的資料列資料行中顯示的記錄的資料欄位。 GridView，使其嵌入式管理單元的顯示，瀏覽、 排序、 編輯和刪除資料，是一個位元 boxy 其外觀。 此外，負責標記 GridView 的結構固定的它包含 HTML`<table>`與資料表資料列 (`<tr>`) 針對每一筆記錄和表格儲存格 (`<td>`) 的每個欄位。

若要顯示多個記錄時，請提供更大的外觀和呈現的標記中的自訂，ASP.NET 2.0 提供 DataList 和中繼器控制項 (這兩種也是用於 ASP.NET 版本 1.x)。 DataList 和中繼器控制項轉譯其內容使用範本，而不是 BoundFields CheckBoxFields，ButtonFields，等等。 GridView，像是在 DataList 呈現為 HTML `<table>`，但允許多個資料來源記錄的每個資料表資料列顯示。 中繼器，相反地，會將比您明確指定，以及您需要精確控制發出的標記時的理想候選項目沒有其他標記呈現。

在下一次數十個或因此教學課程中，我們會探討建置通用的報告模式使用 DataList 和中繼器控制項中，開始使用這些控制項範本顯示資料的基本概念。 我們會看到如何格式化這些控制項如何修改資料來源中的記錄 DataList、 主要/詳細資料的常見案例、 方式編輯及刪除資料，配置如何分頁的記錄，依此類推。

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>步驟 1： 加入 DataList 和中繼器教學課程的 Web 網頁

開始本教學課程之前，可讓 s 先花點時間加入 ASP.NET 網頁，我們需要為本教學課程和下一步的幾個教學課程，以顯示使用 DataList 和中繼器的資料處理。 藉由建立新的資料夾中名為專案啟動`DataListRepeaterBasics`。 接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![建立 DataListRepeaterBasics 資料夾，以及新增的教學課程的 ASP.NET 頁面](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**圖 1**： 建立`DataListRepeaterBasics`資料夾並加入的教學課程的 ASP.NET 網頁


開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。 此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


若要有項目符號清單顯示 DataList 和中繼器教學課程中，我們將會建立，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並新增自訂按鈕的站台對應節點標記後面加入下列標記：


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**圖 3**： 更新包含新的 ASP.NET 網頁站台對應


## <a name="step-2-displaying-product-information-with-the-datalist"></a>步驟 2： 顯示產品資訊的資料清單

類似於在 FormView，DataList 控制項 s 轉譯輸出取決於樣板，而非 BoundFields、 CheckBoxFields，等等。 不同於在 FormView 中，DataList 被設計來顯示一筆記錄，而非單獨一組。 可讓 s 開始此教學課程會查看繫結至資料清單的產品資訊。 先開啟`Basics.aspx`頁面`DataListRepeaterBasics`資料夾。 接下來，從 [工具箱] 拖曳至設計工具拖曳資料清單。 如圖 4 所示，然後再指定 DataList 的範本中，設計工具它顯示為灰色方塊。


[![從 [工具箱] 拖曳至設計工具拖曳 DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**圖 4**: DataList 從工具箱拖曳至設計工具拖曳 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


DataList s 從智慧標籤，加入新 ObjectDataSource 並將它設定為使用`ProductsBLL`類別的`GetProducts`方法。 因為我們重新建立唯讀 DataList 在本教學課程中，設定下拉式清單，為 （無） 在精靈的插入、 更新和刪除索引標籤。


[![選擇建立新的 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**圖 5**： 選擇建立新的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![設定使用 ProductsBLL 類別 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**圖 6**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![擷取的所有產品使用 GetProducts 方法的相關資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**圖 7**： 擷取資訊有關所有產品使用`GetProducts`方法 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Visual Studio 會自動建立後設定 ObjectDataSource 並將它與透過其智慧標籤在 DataList，`ItemTemplate`中顯示的名稱和值的資料來源傳回的每個資料欄位 DataList (請參閱標記下方）。 此預設`ItemTemplate`的外觀是相同的資料來源繫結至 FormView 透過設計工具時自動建立的範本。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> 請記得當透過 FormView s 智慧標籤在 FormView 控制項繫結資料來源，Visual Studio 建立`ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 DataList，不過，與只`ItemTemplate`建立。 這是因為在 DataList 沒有相同的內建編輯和插入 FormView 所提供的支援。 在 DataList 並包含編輯和刪除相關的事件，並編輯和刪除支援可加入的程式碼，但沒有 s 位元不簡單的方塊外做為提供的支援在 FormView。 我們會看到如何包含編輯和刪除與 DataList 的支援，在未來的教學課程。


可讓 s 花點時間改善此範本的外觀。 而不是顯示的所有資料欄位，可讓 s 只會顯示產品的名稱、 供應商、 類別、 每個單位和單價的數量。 此外，可讓 s 顯示中的名稱`<h4>`標題，並使用其餘欄位的版面配置設定`<table>`標題之下。

若要進行這些變更，您可以使用範本編輯功能的設計工具從 DataList s 智慧標籤按一下 [編輯樣板] 連結，或者您可以修改的範本手動透過頁面 s 宣告式語法。 如果您在設計工具中使用 [編輯樣板] 選項，產生標記可能未下列標記完全符合，但是當透過檢視瀏覽器應該看起來很像螢幕擷取畫面圖 8 所示。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> 以上範例使用標籤 Web 控制項的`Text`資料繫結語法的值指派給屬性。 或者，我們無法省略標籤發生，請輸入在資料繫結語法。 也就是說，而不是使用`<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`我們原也可以改為使用宣告式語法`<%# Eval("CategoryName") %>`。


離開標籤 Web 控制項中，不過，提供兩個好處。 首先，它會提供您更輕鬆的方式格式化資料，所依據的資料，我們會看到下一個教學課程中。 其次，設計工具不 t 顯示宣告式資料繫結語法中的 [編輯樣板] 選項，會出現在某些 Web 控制項外。 相反地，編輯樣板介面設計來加速對使用靜態的標記，以及 Web 控制項，並假設任何資料繫結會透過 [編輯資料繫結] 對話方塊，可從 Web 控制項的智慧標籤。

因此，當使用 DataList，可提供編輯的範本，透過設計工具的選項，我想要使用標籤 Web 控制項，使內容可以透過編輯樣板介面存取。 我們會發現，中繼器需要從原始碼檢視編輯的範本的內容。 因此，除非我知道我要格式化的製作中繼器的範本通常，我將省略標籤 Web 控制項時資料的外觀繫結文字為基礎的程式設計邏輯。


[![每個產品的輸出是呈現使用 DataList ItemTemplate s](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**圖 8**： 每個產品的輸出是呈現使用 DataList s `ItemTemplate` ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>步驟 3： 改善 datalist 的外觀

GridView，像是在 DataList 提供了許多樣式相關的屬性，例如`Font`， `ForeColor`， `BackColor`， `CssClass`， `ItemStyle`， `AlternatingItemStyle`， `SelectedItemStyle`，依此類推。 當使用 GridView 和 DetailsView 控制項，我們建立面板檔案中的`DataWebControls`預先定義的佈景主題`CssClass`這兩個控制項的屬性和`CssClass`及其子屬性的數個屬性 (`RowStyle``HeaderStyle`等等)。 可讓 s 執行相同的資料清單。

中所述[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教學課程，面板檔案指定的 Web 控制項的預設外觀相關屬性; 佈景主題的外觀、 CSS、 影像和 JavaScript 檔案定義的集合網站有特定外觀與風格。 中*顯示資料與 ObjectDataSource*教學課程中，我們建立`DataWebControls`佈景主題 (這會實作成內的資料夾`App_Themes`資料夾)，有，目前，兩個面板檔案-`GridView.skin`和`DetailsView.skin`. 可讓 s 加入第三個面板檔案，以指定在 DataList 的預先定義的樣式設定。

若要加入面板檔案，以滑鼠右鍵按一下`App_Themes/DataWebControls`資料夾中，選擇 加入新項目，並從清單中選取的面板檔案選項。 將檔案命名為 `DataList.skin`。


[![建立名為 DataList.skin 新面板檔案](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**圖 9**： 建立新面板檔案命名為`DataList.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


使用下列標記`DataList.skin`檔案：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

這些設定會將相同的 CSS 類別指派給適當的 DataList 屬性中，如與 GridView 和 DetailsView 控制項搭配使用。 這裡使用的 CSS 類別`DataWebControlStyle`， `AlternatingRowStyle`，`RowStyle`等等中定義`Styles.css`檔案，且會加入先前的教學課程。

透過這個面板檔案，DataList 的外觀會更新設計工具 （您可能需要重新整理設計工具檢視，若要查看效果新面板檔案; 的 檢視 功能表，選擇 重新整理）。 如圖 10 所示，每個替代產品有淺粉紅色的背景色彩。


[![建立名為 DataList.skin 新面板檔案](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**圖 10**： 建立新面板檔案命名為`DataList.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>步驟 4： 瀏覽 DataList s 其他範本

除了`ItemTemplate`，DataList 支援六個其他選用的範本：

- `HeaderTemplate` 如果提供，將標頭資料列加入至輸出，而且用來呈現此資料列
- `AlternatingItemTemplate` 用來呈現替代項目
- `SelectedItemTemplate` 用來呈現選取的項目。選取的項目是項目索引的對應至 DataList s [ `SelectedIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` 用來呈現正在編輯的項目
- `SeparatorTemplate` 如果提供，將每個項目之間的分隔符號與用來呈現這個分隔符號
- `FooterTemplate` -如果提供，頁尾資料列加入至輸出，而且用來呈現此資料列

當指定`HeaderTemplate`或`FooterTemplate`，DataList 將額外的頁首或頁尾資料列加入至轉譯的輸出。 類似的 GridView 的頁首和頁尾資料列、 頁首和頁尾中 DataList 未繫結至資料。 因此，在任何資料繫結語法`HeaderTemplate`或`FooterTemplate`嘗試存取繫結的資料就會傳回空白的字串。

> [!NOTE]
> 如我們所見中[GridView 的頁尾中顯示摘要資訊](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)教學課程中，雖然頁首及頁尾資料列不 t 支援資料繫結語法，資料特有的資訊直接插入這些資料列GridView 的`RowDataBound`事件處理常式。 這項技術可以用來同時計算執行總計或其他資訊與資料繫結至控制項，以及將該資訊指派給頁尾。 此概念可以套用至 DataList 和中繼器控制項中。唯一的差別是 DataList 和中繼器建立事件處理常式`ItemDataBound`事件 (而不是針對`RowDataBound`事件)。


我們的範例，讓 s 有標題顯示在 DataList 的結果中的最上方的產品資訊`<h3>`標題。 若要達成此目的，將`HeaderTemplate`為適當的標記。 從設計工具中，這可藉由按一下 編輯範本中的連結 DataList s 智慧標籤上，選擇標頭的範本，從下拉式清單中，並輸入之後挑選標題 3 選項，從下拉式清單的樣式的文字清單 （請參閱圖 11）。


[![新增 HeaderTemplate 文字產品資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**圖 11**： 新增`HeaderTemplate`文字產品資訊 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


或者，這可以加入以宣告方式輸入中的下列標記`<asp:DataList>`標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

若要加入的每個產品清單之間的位元，可讓 s 新增`SeparatorTemplate`，其中包含每個區段之間的分隔線。 水平尺規標記 (`<hr>`)，將這類分割線。 建立`SeparatorTemplate`，使其具有下列標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> 像`HeaderTemplate`和`FooterTemplates`、`SeparatorTemplate`未繫結至任何記錄從資料來源，因此不能直接存取的資料來源繫結至資料清單的記錄。


之後此新增功能，當檢視透過瀏覽器頁面看起來應該類似於圖 12。 請注意，標頭資料列和每個產品清單之間的線。


[![在 DataList 包含標頭資料列和每個產品清單之間的水平尺規](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**圖 12**: DataList 包含標頭資料列和水平規則之間每個產品清單 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>步驟 5： 轉譯特定的標記與在中繼器控制項

若要檢視/來源從瀏覽器瀏覽圖 12 DataList 範例時，您會看到 DataList 發出 HTML`<table>`包含資料表資料列 (`<tr>`) 與單一資料表儲存格 (`<td>`) 的每個項目繫結至DataList。 事實上，此輸出中，是相同功能的 GridView 單一 TemplateField 從發出。 我們會看到在未來的教學課程中，DataList 允許進一步自訂輸出，讓我們能夠顯示每個資料表資料列的多個資料來源記錄。

如果您不想要發出 HTML `<table>`，但是嗎？ 產生資料 Web 控制項的標記總數和已完整控制，我們必須使用在中繼器控制項。 在 DataList，像是中繼器範本為基礎建構。 中繼器，不過，僅提供下列五個範本：

- `HeaderTemplate` 如果提供，，將指定的標記之前的項目
- `ItemTemplate` 用來呈現項目
- `AlternatingItemTemplate` 如果提供，，用來呈現替代項目
- `SeparatorTemplate` 如果提供，，將每個項目之間指定的標記
- `FooterTemplate` -如果提供，會在項目後面加入指定的標記

在 ASP.NET 中 1.x 中繼器控制項通常用來顯示其資料是來自某個資料來源項目符號清單。 在這種情況下，`HeaderTemplate`和`FooterTemplates`會包含在開頭和結尾`<ul>`標記，分別而`ItemTemplate`會包含`<li>`具有資料繫結語法的項目。 這種方法可以仍可用於 ASP.NET 2.0 中的兩個範例中[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程：

- 在`Site.master`主版頁面中繼器用來顯示項目符號清單 （基本報表功能、 篩選報表，自訂格式和等等） 的頂層站台對應內容; 另一個、 巢狀中繼器用來顯示的子系區段最上層區段
- 在`SectionLevelTutorialListing.ascx`，中繼器用來顯示目前的站台對應區段的子系區段項目符號清單

> [!NOTE]
> ASP.NET 2.0 導入新[BulletedList 控制項](https://msdn.microsoft.com/library/ms228101.aspx)，它可以才能顯示簡單的項目符號清單繫結至資料來源控制項。 與設定 BulletedList 控制項我們不需要指定任何相關清單 HTML 中。相反地，我們只是指出要顯示為每個清單項目文字的資料欄位。


中繼器當做 catch Web 控制項的所有資料。 如果不是產生所需的標記的現有控制項，在中繼器控制項可用。 為了說明使用中繼器，可讓 s 已在步驟 2 中建立產品資訊 DataList 上方顯示的類別目錄的清單。 O d e s 特別的是，具有單一資料列 HTML 中顯示類別目錄`<table>`且顯示為資料表中的資料行的每個分類。

若要完成此動作，啟動拖曳中繼器控制項從工具箱拖曳至設計工具中，產品資訊 DataList 上方。 如同 DataList 中繼器一開始會顯示為灰色方塊之前已定義的範本。


[![中繼器加入設計工具](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**圖 13**: 中繼器加入設計工具 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


中繼器 s 智慧標籤在該處 s 只有一個選項： 選擇資料來源。 選擇建立新的 ObjectDataSource，並將它設定為使用`CategoriesBLL`類別的`GetCategories`方法。


[![建立新的 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**圖 14**： 建立新的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![設定使用 CategoriesBLL 類別 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**圖 15**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![擷取所有使用 GetCategories 方法的類別目錄的相關資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**圖 16**： 擷取資訊有關所有類別使用的`GetCategories`方法 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


不同於在 DataList，Visual Studio 不會自動建立 ItemTemplate 中繼器的繫結至資料來源之後。 此外，中繼器的範本無法透過設計工具設定，且必須以宣告方式指定。

若要以單一資料列顯示分類`<table>`具有每個類別資料行，我們需要中繼器發出類似下列標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

因為`<td>Category X</td>`文字會重複出現，這會在中繼器的 ItemTemplate 中的部分。 它的前面顯示的標記`<table><tr>`-將會放置於`HeaderTemplate`時的結束標記- `</tr></table>` -會置於`FooterTemplate`。 若要輸入這些範本設定，請移至 ASP.NET 網頁的宣告式部分按一下左上角的 [來源] 按鈕並輸入下列語法：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

中繼器發出其範本、 沒有其他項目，執行任何動作所指定的精確標記。 圖 17 顯示透過瀏覽器檢視時，才會進行中繼器的輸出。


[![單一資料列 HTML&lt;資料表&gt;列出個別的資料行中的每個類別目錄](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**圖 17**: 單一資料列 HTML`<table>`列出個別的資料行中的每個類別目錄 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>步驟 6： 改善中繼器的外觀

因為中繼器發出精確地將其範本所指定的標記，應該就不會對有中繼器沒有樣式相關屬性。 若要改變中繼器所產生內容的外觀，我們必須手動將所需的 HTML 或 CSS 內容直接加入 s 中繼器範本。

我們的範例，可讓 s 有類似的背景色彩，替代 DataList 中交替資料列的類別資料行。 若要達成此目的，我們必須指派`RowStyle`每個中繼器項目的 CSS 類別和`AlternatingRowStyle`透過每個替代中繼器項目的 CSS 類別`ItemTemplate`和`AlternatingItemTemplate`範本，就像這樣：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

可讓 s 也將標頭資料列加入至與產品類別目錄的文字輸出。 因為我們不知道資料行數目我們產生`<table>`由所組成，最簡單的方式產生，保證跨越所有欄標頭資料列就是使用*兩個* `<table>` s。 第一個`<table>`標頭資料列和資料列會包含第二個，單一資料列會包含兩個資料列`<table>`，系統中有每個類別目錄的資料行。 也就是我們想要發出下列標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

下列`HeaderTemplate`和`FooterTemplate`產生所需的標記：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

圖 18 顯示中繼器進行這些變更之後。


[![背景色彩的替代類別資料行，並包含標頭資料列](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**圖 18**: 類別資料行替代背景色彩和包括標頭資料列中 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>總結

GridView 控制項可讓您輕鬆地顯示，而編輯、 刪除、 排序及瀏覽資料、 外觀也非常 boxy 類似格線。 如需更多控制外觀的詳細資訊，我們需要 DataList 或中繼器控制項中開啟。 這兩個這些控制項顯示一的組記錄使用範本，而不 BoundFields、 CheckBoxFields，等等。

在 DataList 呈現為 HTML `<table>` ，依預設，會顯示每個資料來源記錄的單一資料表資料列，就像單一 TemplateField 的 GridView。 不過，我們將會看到在未來的教學課程中，DataList 並允許多個記錄的每個資料表資料列顯示。 中繼器，相反地，嚴格發出其範本; 中指定的標記它不會新增任何額外的標記，並因此通常用來顯示資料的 HTML 項目以外`<table>`（例如，在項目符號清單）。

DataList 和中繼器提供更靈活地轉譯的輸出，而他們缺乏許多內建在 GridView 中找到的功能。 我們將檢驗即將推出的教學課程中，這其中部分功能可以插入回而不需要太多工作，但不要繼續記住，使用在 DataList 或中繼器 GridView 而未限制而不必實作這些功能，您可以使用的功能您自己。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 會導致此教學課程中的檢閱者已 Yaakov Ellis、 Liz Shulok、 袁 Schmidt 和 Stacy 公園。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
