---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: 使用 DataList 與重複項控制項 (C#) 顯示資料 |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中，我們有使用 GridView 控制項來顯示資料。 開始本教學課程中，我們建置與常見報告模式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: ca585c51ae12be2395897044f6d0fd1857d3e357
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382698"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>使用 DataList 與重複項控制項 (C#) 顯示資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe)或[下載 PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> 在先前的教學課程中，我們有使用 GridView 控制項來顯示資料。 開始本教學課程中，我們探討建置常見的報告模式使用 DataList 與重複項控制項中，開始使用這些控制項顯示資料的基本概念。


## <a name="introduction"></a>簡介

在所有的範例，在過去 28 教學課程中，如果我們需要以顯示來自去 GridView 控制項的資料來源的多個記錄。 GridView 呈現之資料來源中的每一筆記錄的資料列資料行中顯示記錄的資料欄位。 而 GridView 輕鬆顯示，請瀏覽、 排序、 編輯和刪除資料，其外觀是有點 boxy。 此外，標記負責 GridView 的結構是固定格式包括 HTML`<table>`的資料表資料列 (`<tr>`) 的每一筆記錄和表格儲存格 (`<td>`) 每個欄位。

若要顯示多筆記錄時，請提供更高的外觀和呈現的標記中的自訂，ASP.NET 2.0 提供的 DataList 與重複項控制項 (這兩者也都可使用 ASP.NET 版本 1.x)。 DataList 與重複項控制項呈現使用範本，而不是 BoundFields CheckBoxFields，ButtonFields，其內容等等。 GridView，像 DataList 呈現為 HTML `<table>`，但可提供多個資料來源的記錄顯示每個資料表資料列。 中繼器，相反地，呈現沒有額外的標記比什麼您明確指定，且您需要精確地控制送出的標記時的理想候選項目。

透過接下來數十個教學課程中，我們將探討建置常見的報告模式使用 DataList 與重複項控制項中，開始使用這些控制項範本顯示資料的基本概念。 我們會看到如何格式化這些控制項，如何修改資料來源中的記錄 DataList、 主版/詳細資料的常見案例、 方式，來編輯和刪除資料，配置如何逐頁查看記錄，並依此類推。

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>步驟 1： 加入 DataList 和 Repeater 教學課程的 Web 網頁

在開始本教學課程之前，可讓 s 先花點時間，將 ASP.NET 頁面，我們需要為本教學課程和下一步 的幾個教學課程，以因應顯示使用 DataList 與重複項的資料。 藉由建立新的資料夾中名為專案啟動`DataListRepeaterBasics`。 接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![建立 DataListRepeaterBasics 資料夾，並新增這些教學課程的 ASP.NET 網頁](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**圖 1**： 建立`DataListRepeaterBasics`資料夾並新增教學課程的 ASP.NET 頁面


開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。 此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，會列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


若要有項目符號清單顯示 DataList 與重複項教學課程會建立我們，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並新增自訂按鈕的站台對應節點標記之後新增下列標記：


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁


## <a name="step-2-displaying-product-information-with-the-datalist"></a>步驟 2： 顯示與產品資訊

類似於 FormView，DataList 控制項 s 轉譯的輸出取決於範本而非 BoundFields、 CheckBoxFields，等等。 不同於 FormView，DataList 被設計來顯示一份記錄，而不是一個單獨的帳戶。 可讓 s 開始本教學課程，了解繫結至 DataList 的產品資訊。 首先開啟`Basics.aspx`頁面中`DataListRepeaterBasics`資料夾。 接下來，從 [工具箱] 拖曳至設計工具拖曳 DataList。 圖 4 所示，再指定 DataList 的範本設計工具它顯示為灰色方塊。


[![從 [工具箱] 拖曳至設計工具拖曳 DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**圖 4**： 拖曳 DataList 從工具箱拖曳至設計工具 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


DataList s 從智慧標籤、 新增新的 ObjectDataSource 和將它設定為使用`ProductsBLL`類別的`GetProducts`方法。 因為我們在本教學課程中，建立唯讀的 DataList re 中精靈的插入設定下拉式清單中的，為 （無）、 更新和刪除索引標籤。


[![選擇建立新的 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**圖 5**： 選擇建立新的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![設定使用 ProductsBLL 類別 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**圖 6**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![擷取所有的產品使用 GetProducts 方法的相關資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**圖 7**： 擷取資訊有關所有的產品使用`GetProducts`方法 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Visual Studio 會自動建立設定 ObjectDataSource，並將它與透過它的智慧標籤產生關聯之後, `ItemTemplate` DataList 顯示的名稱和值的資料來源傳回每個資料欄位中 (請參閱下列標記）。 此預設`ItemTemplate`外觀完全相同的資料來源繫結至設計工具透過 FormView 時自動建立的範本。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> 您應該記得，在透過 FormView s 智慧標籤的 FormView 控制項，繫結資料來源，Visual Studio 就會建立`ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 使用 DataList，不過，只有`ItemTemplate`建立。 這是因為 DataList 並沒有相同的內建編輯和插入 FormView 所提供的支援。 DataList 並包含編輯和刪除相關的事件，並編輯和刪除支援可加入的程式碼，但沒有 s 稍微沒有簡單的立即可用的支援為使用 FormView。 我們會看到如何包含編輯和刪除具有 DataList 的支援，在未來的教學課程。


可讓 s 花一點時間改善此範本的外觀。 而不是顯示的所有資料欄位，可讓 s 只顯示產品名稱、 供應商、 類別、 每個單位和單價的數量。 此外，可讓 s 顯示中的名稱`<h4>`標題，並使用其餘欄位的版面配置設定`<table>`標題之下。

若要進行這些變更，您可以使用範本編輯功能，在設計工具中，從 DataList s 智慧標籤按一下 [編輯範本] 連結，或者您可以修改範本，以手動方式透過頁面 s 宣告式語法。 如果您在設計工具中使用 編輯範本 選項，產生的標記可能不會完全，下列標記符合，但當透過檢視瀏覽器應該看起來非常類似的螢幕擷取畫面的 圖 8 所示。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> 上述範例使用 Label Web 控制項`Text`資料繫結語法的值指派給屬性。 或者，我們可以省略標籤，請輸入只是資料繫結語法中。 也就是說，而不是使用`<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`我們也可以改為使用宣告式語法`<%# Eval("CategoryName") %>`。


離開 Label Web 控制項中，不過，提供兩個優點。 首先，它會提供您更輕鬆的方式格式化資料，所依據的資料，如在下一個教學課程中稍後所示。 第二，在設計工具不 t 顯示宣告式資料繫結語法中的 [編輯範本] 選項，會出現在某些 Web 控制項外。 相反地，編輯樣板介面設計來協助使用靜態的標記，以及 Web 控制項，並假設任何資料繫結會透過 [編輯資料繫結] 對話方塊中，因此可從 Web 控制項的智慧標籤。

因此，資料清單，可提供選擇編輯透過設計工具的範本，使用時我偏好使用 Label Web 控制項，使其內容可透過編輯樣板介面存取。 如我們所見，Repeater 會需要從原始碼檢視編輯的範本的內容。 因此，除非我知道我要格式化的製作 Repeater 的範本，我通常會省略 Label Web 控制項時資料的外觀會結合程式設計邏輯為基礎的文字。


[![每個產品的輸出是轉譯使用 DataList 的 ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**圖 8**： 每個產品的輸出是轉譯使用 DataList s `ItemTemplate` ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>步驟 3： 改善 DataList 的外觀

如同 GridView、 DataList 提供數個與樣式相關屬性，例如`Font`， `ForeColor`， `BackColor`， `CssClass`， `ItemStyle`， `AlternatingItemStyle`， `SelectedItemStyle`，依此類推。 當使用 GridView 和 DetailsView 控制項，我們會建立面板檔案中的`DataWebControls`預先定義的佈景主題`CssClass`這兩個控制項的屬性和`CssClass`及其子屬性的數個屬性 (`RowStyle``HeaderStyle`等等)。 可讓執行相同動作的 DataList 的 s。

中所述[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教學課程，面板檔案指定的 Web 控制項的預設外觀相關屬性，主題是定義的面板、 CSS、 影像和 JavaScript 檔案的集合網站特定外觀及操作。 在 *顯示的資料與 ObjectDataSource*教學課程中，我們建立`DataWebControls`佈景主題 (這會實作為內的資料夾`App_Themes`資料夾) 具有，目前，兩個面板檔案-`GridView.skin`和`DetailsView.skin`. 可讓 s 新增第三個面板檔案來指定 DataList 的預先定義的樣式設定。

若要新增面板檔案，以滑鼠右鍵按一下`App_Themes/DataWebControls`資料夾中，選擇 加入新項目，並從清單中選取面板檔案選項。 將檔案命名為 `DataList.skin`。


[![建立名為 DataList.skin 新面板檔案](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**圖 9**： 建立新的面板檔案命名`DataList.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


使用下列標記`DataList.skin`檔案：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

這些設定會將相同的 CSS 類別指派給適當的 DataList 屬性中，為使用 GridView 和 DetailsView 控制項所使用。 此處所使用的 CSS 類別`DataWebControlStyle`， `AlternatingRowStyle`，`RowStyle`等中定義`Styles.css`檔案，並在先前的教學課程中所新增。

此面板檔案加入，DataList 外觀會更新在設計工具中 （您可能需要重新整理設計工具檢視，若要查看效果，新的面板檔案; 從 檢視 功能表，選擇 重新整理）。 如 [圖 10] 所示，每個替代的產品都有亮粉紅色的背景色彩。


[![建立名為 DataList.skin 新面板檔案](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**圖 10**： 建立新的面板檔案命名`DataList.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>步驟 4： 瀏覽的 DataList s 其他範本

除了`ItemTemplate`，DataList 支援六種其他選用的範本：

- `HeaderTemplate` 如果提供，將標頭資料列加入至輸出，以及用來呈現此資料列
- `AlternatingItemTemplate` 用來呈現替代項目
- `SelectedItemTemplate` 用來呈現已選取的項目;選取的項目是其索引對應至 DataList s 的項目[`SelectedIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` 用來呈現要編輯的項目
- `SeparatorTemplate` 如果提供，將每個項目之間的分隔符號與用來呈現此分隔符號
- `FooterTemplate` -如果提供，頁尾資料列加入至輸出，以及用來呈現此資料列

指定時`HeaderTemplate`或`FooterTemplate`，DataList 將額外的頁首或頁尾資料列加入至轉譯的輸出。 像是 GridView 的頁首和頁尾資料列、 頁首和頁尾中 DataList 是不繫結至資料。 因此，在任何資料繫結語法`HeaderTemplate`或`FooterTemplate`嘗試存取繫結的資料將會傳回空白的字串。

> [!NOTE]
> 如我們在中所見[GridView s 頁尾顯示摘要資訊](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)教學課程中，雖然頁首及頁尾資料列不 t 支援資料繫結語法，特定資料的資訊可以直接插入這些資料列GridView 的`RowDataBound`事件處理常式。 這項技術可以用來同時計算執行總計或其他資訊與資料繫結至控制項，以及將該資訊指派給頁尾。 這個相同的概念可以套用至 DataList 與重複項控制項;唯一的差別是，對於 DataList 與重複項建立的事件處理常式`ItemDataBound`事件 (而不是如`RowDataBound`事件)。


我們的範例，可讓 s 有標題中的 DataList 的結果頂端顯示的產品資訊`<h3>`標題。 若要達成此目的，將新增`HeaderTemplate`與適當的標記。 從設計工具中，這可藉由按一下 DataList s 智慧標籤中的 編輯範本 連結、 選擇 標頭 範本，從下拉式清單中，並挑選樣式下拉式清單的標題 3 選項後的文字輸入清單 （請參閱 圖 11）。


[![新增 HeaderTemplate 的文字產品資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**圖 11**： 新增`HeaderTemplate`文字產品資訊 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


或者，這可以是以宣告方式新增輸入中的下列標記`<asp:DataList>`標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

若要新增的每個產品清單之間的空間，可讓新增的 s `SeparatorTemplate` ，其中包含每個區段之間的線條。 水平尺規標記 (`<hr>`)，將這類分割線。 建立`SeparatorTemplate`，使其具有下列標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> 像是`HeaderTemplate`並`FooterTemplates`，則`SeparatorTemplate`未繫結至任何記錄從資料來源，因此不能直接存取的資料來源繫結至 DataList 的記錄。


後此新增功能，檢視透過瀏覽器頁面時它看起來應該類似 圖 12。 請注意，標頭資料列和每個產品清單之間的線。


[![DataList 包含標頭資料列和每個產品清單之間的水平尺規](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**圖 12**: DataList 包含標頭資料列和水平規則之間每個產品清單 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>步驟 5： 轉譯特定的標記與重複項控制項

如果瀏覽 圖 12 的 DataList 範例時，您可以執行從瀏覽器的檢視/原始檔，您會看到 DataList 會發出 HTML`<table>`包含資料表資料列 (`<tr>`) 包含單一資料表資料格 (`<td>`) 針對每個項目繫結至DataList。 此輸出中，實際上是相同項目會從單一的 TemplateField 與 GridView 發出。 我們將在未來的教學課程中所看到的 DataList 確實允許進一步自訂的輸出，讓我們可以顯示每個資料表資料列的多個資料來源記錄。

如果您不希望在發出 HTML `<table>`，不過嗎？ 產生資料 Web 控制項的標記總且完整控制，我們必須使用 Repeater 控制項。 像 DataList 和 Repeater 是根據範本來建構。 重複項，不過，只會提供下列五個範本：

- `HeaderTemplate` 如果提供，，將指定的標記之前的項目
- `ItemTemplate` 用來呈現項目
- `AlternatingItemTemplate` 如果提供，，用來呈現替代項目
- `SeparatorTemplate` 如果提供，，將每個項目之間指定的標記
- `FooterTemplate` -如果提供，可將指定的標記新增的項目之後

在 ASP.NET 1.x 中，控制項通常用來顯示項目符號清單，其中的資料來自特定資料來源的重複項。 在此情況下，`HeaderTemplate`並`FooterTemplates`會包含開頭和結尾`<ul>`標記，分別，而`ItemTemplate`會包含`<li>`具有資料繫結語法的項目。 這種方法可以仍在 ASP.NET 2.0 中使用，如我們在兩個範例中所見[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程：

- 在 `Site.master`主版頁面上，重複項用來顯示項目符號清單 （基本報告、 篩選報表，自訂格式和等等） 時，頂層站台對應內容; 另一個、 巢狀的重複項用來顯示的子系區段最上層區段
- 在  `SectionLevelTutorialListing.ascx`，重複項用來顯示目前的站台對應區段的子系區段的項目符號清單

> [!NOTE]
> ASP.NET 2.0 引進的新[BulletedList 控制項](https://msdn.microsoft.com/library/ms228101.aspx)，它可以繫結至資料來源控制項以顯示簡單的項目符號清單。 與 BulletedList 控制項我們不需要指定任何清單相關 HTML 中;相反地，我們只是表示資料欄位顯示為每個清單項目的文字。


中繼器當做 catch Web 控制項的所有資料。 如果沒有現有的控制項，就會產生所需的標記，就可以使用 Repeater 控制項。 若要說明如何使用重複項，可讓 s 已在步驟 2 中建立產品資訊 DataList 上方顯示的類別目錄的清單。 特別的是，可讓 s 已顯示在單一資料列的 HTML 中的類別`<table>`而顯示為資料表的資料行的每個類別。

若要達成此目的，先拖曳 Repeater 控制項從工具箱拖曳至設計工具中，上述產品資訊 DataList。 如同 DataList 和 Repeater 一開始會顯示為灰色方塊之前已定義其範本。


[![新增至設計工具的重複項](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**圖 13**： 加入設計工具中的重複項 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


在 Repeater s 智慧標籤的該處 s 只有一個選項： 選擇資料來源。 選擇建立新的 ObjectDataSource，並將它設定為使用`CategoriesBLL`類別的`GetCategories`方法。


[![建立新的 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**圖 14**： 建立新的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![設定使用 CategoriesBLL 類別 ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**圖 15**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![擷取所有使用 GetCategories 方法的類別的相關資訊](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**圖 16**： 擷取資訊有關所有類別使用的`GetCategories`方法 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


不像 DataList 和 Visual Studio 不會自動建立 ItemTemplate Repeater 的繫結至資料來源之後。 此外，重複項的範本無法透過設計工具來設定，而且必須以宣告方式指定。

若要以單一資料列中顯示類別`<table>`具有每個類別資料行，我們需要發出標記類似下面的 Repeater:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

因為`<td>Category X</td>`文字方塊會重複出現，這會顯示中繼器的 ItemTemplate 中的部分。 出現在它前面的標記`<table><tr>`-將會置於`HeaderTemplate`時的結束標記- `</tr></table>` -會放在`FooterTemplate`。 若要輸入這些範本設定，請移至 ASP.NET 網頁的宣告式部分按一下左下角的 [來源] 按鈕並輸入下列語法：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Repeater 會發出其範本，沒有其他項目，執行任何動作所指定的精確標記。 [圖 17] 顯示透過瀏覽器檢視時，才會進行 Repeater 的輸出。


[![單一資料列的 HTML&lt;資料表&gt;列出個別的資料行中的每個類別目錄](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**圖 17**: 單一資料列 HTML`<table>`列出個別的資料行中的每個類別目錄 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>步驟 6： 改善中繼器的外觀

中繼器發出精確地將其範本所指定的標記，因為它應該來自一點也不為沒有樣式相關屬性的重複項。 若要改變內容 Repeater 所產生的外觀，我們必須手動將所需的 HTML 或 CSS 內容直接加入 Repeater 的範本。

我們的範例，可讓 s 有替代的背景色彩，例如，DataList 中交替資料列的類別資料行。 若要達成此目的，我們需要指派`RowStyle`至每一個重複項目的 CSS 類別和`AlternatingRowStyle`CSS 類別，透過每個替代中繼器項目的`ItemTemplate`和`AlternatingItemTemplate`範本，就像這樣：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

可讓 s 也加入具有產品類別目錄的文字輸出中的標頭資料列。 因為我們不知道多少資料行我們產生`<table>`將會包含，產生保證跨越所有資料行的標頭資料列的最簡單方式是使用*兩個* `<table>` s。 第一個`<table>`標頭資料列和資料列會包含第二個，單一資料列會包含兩個資料列`<table>`，有系統中的每個類別目錄的資料行。 也就是我們想要發出下列標記：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

下列`HeaderTemplate`和`FooterTemplate`產生所需的標記：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

[圖 18] 顯示中繼器進行這些變更之後。


[![背景色彩的替代類別資料行，並包含標頭資料列](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**圖 18**: 類別資料行替代背景色彩] 及 [包含標頭資料列中 ([按一下以檢視完整大小的影像](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>總結

雖然 GridView 控制項可讓您輕鬆地顯示，編輯、 刪除、 排序和資料，頁面外觀是非常 boxy 且類似方格的。 進一步控制外觀的詳細資訊，我們需要開啟至 DataList 或 Repeater 控制項。 這兩個這些控制項顯示一組記錄使用範本，而不 BoundFields、 CheckBoxFields，等等。

DataList 呈現為 HTML `<table>` ，根據預設，會顯示每個資料來源記錄單一資料表列的資料，就像使用單一的 TemplateField GridView。 我們將在未來的教學課程中所看到的不過，DataList 並允許每個資料表資料列顯示多筆記錄。 中繼器，相反地，嚴格地發出其範本; 中指定的標記它不會新增任何額外的標記，並因此通常用來在 HTML 項目中顯示資料時，也將以外`<table>`（例如，在項目符號清單）。

雖然 DataList 和 Repeater 提供更多的彈性，其呈現的輸出中，缺乏許多內建在 GridView 中找到的功能。 我們將檢驗在即將推出的教學課程中，部分功能可以不用費多少力氣，插入回到但不要繼續記住，使用 DataList 或 Repeater 代替 GridView 會限制您可以使用而不需要實作這些功能的功能您自己。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Yaakov Ellis、 Liz Shulok、 Randy Schmidt 和 Stacy Park。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
