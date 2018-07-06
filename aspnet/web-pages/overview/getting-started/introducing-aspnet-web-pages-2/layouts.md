---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 簡介 ASP.NET Web Pages-建立一致的版面配置 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何使用配置，以使用 ASP.NET Web Pages 的站台上建立一致的外觀的頁面。 它假設您已完成...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 5641b65ab1053ccc039a94f7a591185ff00ff1c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806542"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>ASP.NET Web Pages 簡介-建立一致的版面配置
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何使用*版面配置*使用 ASP.NET Web Pages 的站台上建立一致的外觀的頁面。 它假設您已完成透過數列[刪除 ASP.NET Web Pages 中的資料庫資料](https://go.microsoft.com/fwlink/?LinkId=251584)。
> 
> 您將學到什麼：
> 
> - 版面配置頁。
> - 如何將版面配置頁具有動態內容。
> - 如何將值傳遞至版面配置頁。


## <a name="about-layouts"></a>關於配置

到目前為止，您已建立的頁面所有已完成，獨立頁面。 它們都屬於相同的站台，但沒有任何通用的項目或一個標準的外觀。

大部分的站台沒有一致的外觀和版面配置。 例如，如果您移至[Microsoft.com/web](https://www.microsoft.com/web/)網站並看一下，您會看到所有頁面都遵守整體的版面配置以及視覺效果佈景主題：

![顯示標頭、 導覽區域中，內容區域和頁尾的配置 Microsoft.com/web site 頁面](layouts/_static/image1.png)

*效率不佳*建立這種版面配置的方式是在每個頁面上個別定義標頭、 導覽列中和頁尾。 將會複製相同的標記每一次。 如果您想要變更的項目 （例如，更新頁尾），您必須個別變更每個頁面。

這時*版面配置頁*進來。 ASP.NET Web Pages 中，您可以定義提供整體的容器，可針對您的網站上的頁面版面配置頁。 例如，[配置] 頁面可以包含標頭、 導覽區域中和頁尾。 [配置] 頁面包含的主要內容在何處的預留位置。

然後，您可以定義包含標記和程式碼，只針對該頁面的個別內容頁面。 內容頁面不一定要完成的 HTML 網頁。他們甚至不必有`<body>`項目。 它們還會告訴 ASP.NET 何種您想要顯示的內容中的 [配置] 頁面的程式碼行。 以下是此關聯性的運作方式大致上顯示的圖片：

![顯示兩個內容頁面和它們所容納的版面配置頁的概念圖](layouts/_static/image2.png)

這種互動很容易了解當您觀看實作示範。 在本教學課程中，您要變更您的影片頁面，以使用版面配置。

## <a name="adding-a-layout-page"></a>新增版面配置頁

首先藉由建立定義的標頭、 頁尾，與主要內容區域的典型版面配置頁面。 在 WebPagesMovies 網站中，新增名為的 CSHTML 頁面 *\_Layout.cshtml*。

前置底線 ( `_` ) 字元很重要。 如果頁面的名稱開頭為底線，ASP.NET 不會直接將該頁面傳送至瀏覽器。 這個慣例可讓您定義所需的網站，但使用者應該不能直接要求的頁面。

您可以將頁面中的內容取代為下列：

[!code-html[Main](layouts/samples/sample1.html)]

如您所見，此標記是只會使用的 HTML`<div>`項目更將三個區段定義頁面加上一個`<div>`保存三個區段的項目。 頁尾包含的 Razor 程式碼： `@DateTime.Now.Year`，這會在網頁中該位置呈現目前的年份。

請注意，名為樣式表的連結*Movies.css*。 樣式表是將會定義實體的項目配置的詳細資料。 您將於稍後建立的。

唯一不尋常的功能，這 *\_Layout.cshtml*頁`@Render.Body()`列。 這是內容位置時，此配置會與另一個頁面合併的預留位置。

## <a name="adding-a-css-file"></a>新增.css 檔案

在頁面上定義的項目實際排列方式 （也就是外觀） 的慣用的方法是使用階層式樣式表 (CSS) 規則。 因此，您會建立 *.css*檔案包含您的新配置的規則。

在 WebMatrix 中，選取您的站台的根目錄。 然後在**檔案**索引標籤的 功能區中，按一下底下的箭號**新增**按鈕，然後按一下**新資料夾**。

![在功能區下新增 [新資料夾] 選項。](layouts/_static/image3.png)

將新資料夾命名*樣式*。

![命名新的資料夾 '樣式'](layouts/_static/image4.png)

在新*樣式*資料夾中，建立名為*Movies.css*。

![建立新的 Movies.css 檔案](layouts/_static/image5.png)

新的內容取代 *.css*以下列檔案：

[!code-css[Main](layouts/samples/sample2.css)]

我們不會說出很多關於這些的 CSS 規則，但要注意兩件事。 其中一個是，除了設定字型與大小，規則使用絕對位置，建立標頭、 頁尾及主要內容區域的位置。 如果您還不熟悉在 CSS 中定位，您可以閱讀[CSS 定位](http://www.w3schools.com/css/css_positioning.asp)W3Schools 站台教學課程。

請注意在底部，我們已複製的樣式規則，原本是個別中定義另一件事*Movies.cshtml*檔案。 這些規則中使用[顯示的資料藉由使用 ASP.NET Web Pages 簡介](https://go.microsoft.com/fwlink/?LinkId=251580)教學課程，以讓`WebGrid`helper 呈現加入至資料表的等量分散的標記。 (如果您打算使用 *.css*檔案樣式定義，您可能也讓整個網站的樣式規則中。)

## <a name="updating-the-movies-file-to-use-the-layout"></a>更新使用版面配置的電影檔

現在您可以更新您的站台，以使用新的版面配置中現有的檔案。 開啟*Movies.cshtml*檔案。 在頂端，做為第一行的程式碼中，新增下列內容：

[!code-csharp[Main](layouts/samples/sample3.cs)]

頁面現在一開始這種方式：

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

這一行程式碼會告知 ASP.NET，當*電影*頁面上執行時，它應該與合併 *\_Layout.cshtml*檔案。

由於*Movies.cshtml*檔案現在會使用版面配置頁面，您可以移除從標記*Movies.cshtml*已經所處理的頁面 *\_Layout.cshtml*檔案。 拿掉`<!DOCTYPE>`， `<html>`，和`<body>`開頭和結尾標記。 取出整個`<head>`項目和它的內容，包含方格中的樣式規則，您現在有這些規則中以來 *.css*檔案。 由於您是在它，就必須變更現有`<h1>`項目`<h2>`項目，您必須`<h1>`已經版面配置頁的項目。 變更`<h2>`「 清單 Movies 」 的文字。

通常您不會有要在內容頁面中的這些類型的變更。 當您開始您的網站的版面配置頁面時，會開始建立內容的頁面，而不需所有這些項目。 在此情況下，不過，您將轉換的獨立頁面使用版面配置，因此沒有進行一點清除的其中一個。

完成之後，當*Movies.cshtml*頁面看起來如下所示：

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>測試版面配置

現在您可以看到版面配置的外觀。 在 WebMatrix 中，以滑鼠右鍵按一下*Movies.cshtml*頁面，然後選取**在瀏覽器中啟動**。 當瀏覽器顯示頁面時，它看起來如下列網頁：

![使用版面配置轉譯的影片頁面](layouts/_static/image6.png)

ASP.NET 已合併到 Movies.cshtml 頁面的內容 *\_Layout.cshtml*頁面上正確的 where`RenderBody`方法。 當然 *\_Layout.cshtml*頁面上參考 *.css*檔案，定義網頁的外觀。

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>更新 AddMovie 頁面，即可使用版面配置

配置的好處是，您可以使用它們的所有頁面在您的網站。 開啟*AddMovie.cshtml*頁面。

您可能記得*AddMovie.cshtml*頁面原本部分 CSS 規則中定義的驗證錯誤訊息外觀。 由於您尚未 *.css*檔案中為您的網站，現在，您可以移動這些規則以 *.css*檔案。 移除*AddMovie.cshtml*檔案，並將它們新增至底部*Movies.css*檔案。 您要移動的下列規則：

[!code-css[Main](layouts/samples/sample6.css)]

現在進行的相同變更各種*AddMovie.cshtml*您對*Movies.cshtml* — 新增`Layout="~/_Layout.cshtml;`並移除現在是多餘的 HTML 標記。 變更`<h1>`項目`<h2>`。 當您完成時，頁面看起來如下列範例：

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

執行網頁。 現在它看起來像下圖：

![使用版面配置來呈現的 [新增影片] 頁面](layouts/_static/image7.png)

您想要在網站中的頁面進行相同的變更 — *EditMovie.cshtml*並*DeleteMovie.cshtml*。 不過，這麼做之前，您可以對另一項變更會更有彈性的配置。

## <a name="passing-title-information-to-the-layout-page"></a>傳遞至版面配置頁的標題資訊

*\_Layout.cshtml*您所建立的頁面具有`<title>`設為 「 我的影片網站 」 的項目。 大部分的瀏覽器會顯示此項目的內容 索引標籤上的文字為：

![在頁面的&lt;title&gt;瀏覽器索引標籤中顯示的項目](layouts/_static/image8.png)

此項目資訊是泛型。 假設您想要更專屬於目前頁面的標題文字。 （標題文字也會使用搜尋引擎來判斷您的頁面是有關。）您可以將資訊傳遞從內容頁面，例如*Movies.cshtml*或是*AddMovie.cshtml*到版面配置頁面，然後使用該自訂版面配置頁面的資訊會呈現。

開啟*Movies.cshtml*頁面上一次。 在頂端的程式碼，加入下面這一行：

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page`物件可供所有 *.cshtml*頁面，並且是基於此目的，也就是它的版面配置和網頁之間共用資訊。

開啟<em>\_Layout.cshtml</em>頁面。 變更`<title>`元素，使它看起來像此標記：

[!code-html[Main](layouts/samples/sample9.html)]

此程式碼會轉譯任何處於`Page.Title`在頁面中的該位置的屬性。

執行*Movies.cshtml*頁面。 目前瀏覽器索引標籤會顯示您傳遞的值為`Page.Title`:

![顯示的標題，以動態方式建立瀏覽器索引標籤](layouts/_static/image9.png)

如果您想在瀏覽器中檢視網頁原始檔。 您可以看到`<title>`項目會轉譯為`<title>List Movies</title>`。

> [!TIP] 
> 
> **Page 物件**
> 
> 實用的功能`Page`是動態的物件 —`Title`屬性不是固定或保留的名稱。 您可以使用*任何*的值名稱`Page`物件。 比方說，您無法輕易傳遞標題使用名為的屬性`Page.CurrentName`或`Page.MyPage`。 唯一的限制是名稱必須遵照可以命名為哪些屬性的一般規則。 （例如，名稱不能包含空格）。
> 
> 您可以將任意數目的值傳遞使用`Page`物件。 如果您想要將影片資訊傳遞至版面配置頁，您無法將值傳遞藉由使用像是`Page.MovieTitle`並`Page.Genre`和`Page.MovieYear`。 （或任何其他您發明了以儲存資訊的名稱。）唯一的需求，這是可能明顯 — 是您必須使用相同的名稱，在 [內容] 頁面和 [配置] 頁面中。
> 
> 您傳遞使用的資訊`Page`物件並不限於只是要顯示在 [配置] 頁面上的文字。 您可以將值傳遞至版面配置 頁面中，，然後在 配置 頁面中的程式碼可以使用值來決定是否要顯示在頁面中，區段什麼 *.css*檔案使用，並依此類推。 您傳入的值`Page`物件會像任何其他值，您使用程式碼。 它只是值源自在 [內容] 頁面中，且會傳遞至版面配置頁。


開啟*AddMovie.cshtml*頁面上，並新增一行至提供的標題的程式碼頂端*AddMovie.cshtml*頁面：

[!code-csharp[Main](layouts/samples/sample10.cs)]

執行*AddMovie.cshtml*頁面。 您會看到那里新的標題：

![顯示新增電影標題，以動態方式建立瀏覽器索引標籤](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>更新其餘的頁面使用的版面配置

現在就可以完成其餘的頁面在您的網站，使其使用新的版面配置。 開啟*EditMovie.cshtml*並*DeleteMovie.cshtml*中開啟，並在每個進行相同的變更。

新增連結版面配置頁面的程式碼行：

[!code-csharp[Main](layouts/samples/sample11.cs)]

加入線條以設定頁面的標題：

[!code-csharp[Main](layouts/samples/sample12.cs)]

或：

[!code-csharp[Main](layouts/samples/sample13.cs)]

移除所有多餘的 HTML 標記 — 基本上，保留內部的位元`<body>`項目 （加上程式碼區塊頂端）。

變更`<h1>`項目成為`<h2>`項目。

當您進行這些變更時，請測試每個，並確定正確顯示，而且標題正確無誤。

## <a name="parting-thoughts-about-layout-pages"></a>版面配置頁的臨別想法

在您建立在本教學課程 *\_Layout.cshtml*頁面上，使用`RenderBody`方法，以從另一個頁面的內容合併。 這是 Web pages 使用版面配置的基本模式。

版面配置頁有我們這裡未涵蓋的其他功能。 例如，您可以巢狀版面配置頁面 — 一個版面配置頁可以接著參考另一個。 巢狀的配置可以是很有用，如果您正在使用的站台需要不同的版面配置的各小節。 您也可以使用其他方法 (例如`RenderSection`) 來設定名為版面配置頁的章節。

版面配置頁的組合並 *.css*檔案是強大的功能。 如您所見下一個教學課程系列中，在 WebMatrix 中您可以建立網站，根據*範本*，可讓您的網站中預先建置的功能。 範本可讓善用版面配置頁及 CSS 來建立的網站的外觀優美且具有的功能，例如功能表。 以下是從範本時，顯示使用版面配置頁面和 CSS 的功能為基礎的站台的 [首頁] 頁面的螢幕擷取畫面：

![顯示標頭、 導覽區域中，內容區域、 選擇性的區段中和登入連結的 WebMatrix 網站範本所建立的版面配置](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>（更新為使用版面配置頁） 的電影頁面的完整清單

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>完成清單 頁面上新增 （已針對版面配置） 的影片頁面

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>頁面的完整清單 （已針對版面配置） 的刪除影片頁面

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>頁面的完整清單 （已針對版面配置） 的編輯影片頁面

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>接下來的下一步

在下一個教學課程中，您將了解如何將您的站台發佈至網際網路，每個人都可以看到它。

## <a name="additional-resources"></a>其他資源

- [建立一致的查詢](https://go.microsoft.com/fwlink/?LinkID=202891)— 文章，使用配置提供更多詳細資料。 它也會說明如何將值傳遞至版面配置頁的顯示或隱藏的部分內容。
- [巢狀版面配置頁面，razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind 部落格如何建立巢狀版面配置頁的範例。 （包括頁面下載）。

> [!div class="step-by-step"]
> [上一頁](deleting-data.md)
> [下一頁](publishing.md)
