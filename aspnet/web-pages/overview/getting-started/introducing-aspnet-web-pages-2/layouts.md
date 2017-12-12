---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "導入 ASP.NET Web Pages-建立一致的版面配置 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何使用配置，以使用 ASP.NET Web Pages 站台建立外觀一致的頁面。 它會假設您已經完成..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>導入的 ASP.NET Web Pages-建立一致的版面配置
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何使用*配置*使用 ASP.NET Web Pages 站台上建立一致的外觀的頁面。 它會假設您已完成透過數列[刪除 ASP.NET Web Pages 中的資料庫資料](https://go.microsoft.com/fwlink/?LinkId=251584)。
> 
> 您將學習：
> 
> - 版面配置頁。
> - 如何結合動態內容的版面配置頁。
> - 如何將值傳遞至版面配置頁。


## <a name="about-layouts"></a>關於配置

到目前為止，您已建立的頁面所有已完成之後，獨立的頁面。 它們都屬於相同的站台，但是沒有任何共同的項目或標準的外觀。

大部分的站台沒有一致的外觀和版面配置。 例如，如果您移至[Microsoft.com/web](https://www.microsoft.com/web/)站台和尋找，，您會看到所有頁面都遵守整體的版面配置，並視覺化佈景主題：

![顯示標頭、 導覽區域中，內容區域和頁尾的配置 Microsoft.com/web 網站頁面](layouts/_static/image1.png)

*效率不佳*建立這個配置的方式是在每個頁面上個別定義標頭、 導覽列中和頁尾。 會被複製的相同標記每次。 如果您想要變更的項目 （例如，更新頁尾），您必須個別變更每個頁面。

這是 where*版面配置頁*出現。 ASP.NET Web Pages 中，您可以定義提供整體的容器網站上頁面的版面配置頁。 例如，[配置] 頁面可以包含標頭、 導覽區域中和頁尾。 [配置] 頁面包含的主要內容前往的地方的預留位置。

接著，您可以定義個別內容的網頁包含標記和程式碼，只有該頁面。 內容頁面不必是完整的 HTML 網頁。即使不一定要有`<body>`項目。 他們也擁有您想要顯示的內容中哪些版面配置頁會告知 ASP.NET 的程式碼行。 以下是此關聯性的運作方式大致上顯示的圖片：

![顯示兩個內容頁面，以及它們所容納的版面配置頁的概念圖](layouts/_static/image2.png)

這種互動很容易了解何時觀看它的運作。 在本教學課程中，您要變更您的電影網頁使用的配置。

## <a name="adding-a-layout-page"></a>加入版面配置頁

您會先建立定義具有頁首、 頁尾及主要內容區域的一般版面配置頁面。 在 WebPagesMovies 網站中，加入名為的 CSHTML 頁面 *\_Layout.cshtml*。

前置底線 ( `_` ) 字元是很重要。 如果頁面名稱會以底線開頭，ASP.NET 不會直接將該頁面傳送至瀏覽器。 這個慣例可讓您定義所需的網站，但使用者不應該能夠直接要求的頁面。

中頁面的內容取代為下面：

[!code-html[Main](layouts/samples/sample1.html)]

如您所見，此標記是只會使用的 HTML`<div>`更定義三個區段中的頁面加一的項目`<div>`保存三個區段的項目。 頁尾包含 Razor 程式碼的位元： `@DateTime.Now.Year`，它會呈現目前年度網頁中的該位置。

請注意，有名稱為樣式表的連結*Movies.css*。 樣式表是將要定義之元素的實際配置的詳細資料。 您將在不久後建立的。

唯一不尋常的功能在這個 *\_Layout.cshtml*頁面是`@Render.Body()`列。 這是的預留位置，內容位置和另一個頁面合併此版面配置時。

## <a name="adding-a-css-file"></a>若要加入.css 檔案

在頁面上定義的項目實際的排列 （亦即，外觀），最好是使用階層式樣式表 (CSS) 規則。 因此，您會建立*.css*檔案，您的新配置的規則。

在 WebMatrix 中，選取您網站的根。 接著在**檔案**] 索引標籤的 [功能區中，按一下底下的箭頭**新增**按鈕，然後按一下**新資料夾**。

![在功能區 下 新資料夾 選項。](layouts/_static/image3.png)

將新的資料夾命名*樣式*。

![命名新的資料夾 '樣式'](layouts/_static/image4.png)

在新*樣式*資料夾中，建立名為*Movies.css*。

![建立新的 Movies.css 檔](layouts/_static/image5.png)

新的內容取代*.css*以下列檔案：

[!code-css[Main](layouts/samples/sample2.css)]

我們將不會假設這些 CSS 規則，但要注意兩件事太多。 其中一個是，除了設定字型和大小，規則使用絕對位置來建立頁首、 頁尾及主要內容區域的位置。 如果您還不熟悉定位在 CSS 中，您可以閱讀[CSS 位置](http://www.w3schools.com/css/css_positioning.asp)教學課程 W3Schools 站台。

請注意在底部，我們已複製原本使用的樣式規則中分別定義另一項作業*Movies.cshtml*檔案。 這些規則中所使用[顯示資料所使用的 ASP.NET Web Pages 簡介](https://go.microsoft.com/fwlink/?LinkId=251580)進行教學課程`WebGrid`helper 呈現新增等量分散到資料表的標記。 (如果您打算使用*.css*檔案樣式定義，您可能也放整個網站的樣式規則中。)

## <a name="updating-the-movies-file-to-use-the-layout"></a>更新要使用的版面配置的電影檔案

現在您可以更新您的網站才能使用新的版面配置中的現有檔案。 開啟*Movies.cshtml*檔案。 在頂端，做為第一行程式碼，加入下列內容：

[!code-csharp[Main](layouts/samples/sample3.cs)]

頁面現在開始這種方式是：

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

這一行程式碼會告知 ASP.NET，當*電影*頁面會執行，它應該與合併 *\_Layout.cshtml*檔案。

因為*Movies.cshtml*檔案現在會使用版面配置頁面，您可以移除從標記*Movies.cshtml*具有所處理的頁面 *\_Layout.cshtml*檔案。 取出`<!DOCTYPE>`， `<html>`，和`<body>`開頭和結尾標記。 取出整個`<head>`項目和其內容，其中包括格線的樣式規則，因為您現在有這些規則中*.css*檔案。 當您在到達時，變更現有`<h1>`元素`<h2>`項目; 您`<h1>`已經 [配置] 頁面中的項目。 變更`<h2>`」 清單電影"的文字。

通常您不會有這些類型的變更，讓內容頁面中。 當您啟動您的站台與版面配置頁面時，會開始建立沒有所有這些項目的內容頁面。 在此情況下，不過，您要轉換的獨立頁面使用版面配置，所以清除的位元的其中一個。

當您完成時， *Movies.cshtml*頁面將如下所示：

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>測試版面配置

現在您可以看到版面配置的外觀。 在 WebMatrix 中，以滑鼠右鍵按一下*Movies.cshtml*頁面，然後選取**瀏覽器中啟動**。 當瀏覽器顯示頁面時，它看起來此頁面：

![使用配置轉譯影片頁面](layouts/_static/image6.png)

ASP.NET 已合併到 Movies.cshtml 頁面的內容 *\_Layout.cshtml*頁面右 where`RenderBody`方法。 和當然 *\_Layout.cshtml*頁面參考*.css*檔案，定義網頁的外觀。

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>更新要使用配置 AddMovie 頁面

配置的好處是，您可以使用它們的所有頁面中您的網站。 開啟*AddMovie.cshtml*頁面。

您可能會記住*AddMovie.cshtml*頁面原來有一些 CSS 規則來定義驗證錯誤訊息的外觀。 由於您*.css*現在您網站的檔案，您可以移動至這些規則*.css*檔案。 移除*AddMovie.cshtml*檔案，並將它們加入到底部*Movies.css*檔案。 您要移動的下列規則：

[!code-css[Main](layouts/samples/sample6.css)]

現在進行中的變更相同的排序*AddMovie.cshtml*進行*Movies.cshtml* — 新增`Layout="~/_Layout.cshtml;`並移除現在是多餘的 HTML 標記。 變更`<h1>`元素`<h2>`。 當您完成時，頁面看起來像本範例中：

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

執行網頁。 現在它看起來像下圖：

!['Add 電影' 的頁面使用配置轉譯](layouts/_static/image7.png)

您想要在站台內的頁面進行類似的變更 — *EditMovie.cshtml*和*DeleteMovie.cshtml*。 不過，在執行之前，您可以對另一項變更會更有彈性的配置。

## <a name="passing-title-information-to-the-layout-page"></a>傳遞至版面配置頁的標題資訊

 *\_Layout.cshtml*您所建立的網頁有`<title>`設為 「 我的電影網站 」 的項目。 大部分的瀏覽器會顯示此項目的內容，做為文字索引標籤上：

![在頁面的&lt;標題&gt;瀏覽器索引標籤中顯示的項目](layouts/_static/image8.png)

此項目資訊是泛型。 假設您想要更精確來目前頁面標題文字。 （標題文字也會使用搜尋引擎來判斷您的頁面是關於。）您可以將資訊傳遞內容的頁面，例如從*Movies.cshtml*或*AddMovie.cshtml*到版面配置頁面，然後使用該資訊自訂版面配置頁面呈現。

開啟*Movies.cshtml*頁面上一次。 在程式碼的頂端加入下列行：

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page`物件可供所有使用*.cshtml*頁面，並且是基於此目的，也就是它的版面配置和網頁之間共用資訊。

開啟*\_Layout.cshtml*頁面。 變更`<title>`讓它看起來像是這個標記的項目：

[!code-html[Main](layouts/samples/sample9.html)]

此程式碼會呈現任何處於`Page.Title`直接在網頁中的該位置的屬性。

執行*Movies.cshtml*頁面。 此時間瀏覽器索引標籤會顯示您傳遞的值為`Page.Title`:

![顯示的標題以動態方式建立瀏覽器索引標籤](layouts/_static/image9.png)

如果您想在瀏覽器中檢視網頁原始檔。 您可以看到`<title>`項目會轉譯為`<title>List Movies</title>`。

> [!TIP] 
> 
> **Page 物件**
> 
> 有用功能`Page`是動態的物件-`Title`屬性不是固定或保留的名稱。 您可以使用*任何*名稱的值`Page`物件。 例如，您可以輕鬆地傳遞標題使用名為的屬性`Page.CurrentName`或`Page.MyPage`。 唯一的限制是名稱必須遵照哪些屬性可以具名的一般規則。 （例如，名稱不能包含空格。）
> 
> 您可以傳遞任何數目的值使用`Page`物件。 如果您想要將電影資訊傳遞至版面配置頁面，您無法將值傳遞使用類似`Page.MovieTitle`和`Page.Genre`和`Page.MovieYear`。 （或任何其他您用來儲存資訊的名稱）。唯一的需求，這是可能明顯 — 是您必須使用相同的名稱，在 [內容] 頁面和 [配置] 頁面中。
> 
> 您使用傳遞的資訊`Page`物件並不限於只是要顯示在 [配置] 頁面上的文字。 您可以將值傳遞至版面配置 頁面和 配置 頁面中的程式碼再使用值來決定是否要顯示的頁面上，區段什麼*.css*檔案使用，等等。 您要傳入的值`Page`物件會像任何其他值，您使用程式碼。 它只是值來自 [內容] 頁面中，而且會傳遞至版面配置頁。


開啟*AddMovie.cshtml*頁面上，然後將行加入至提供的標題的程式碼頂端*AddMovie.cshtml*頁面：

[!code-csharp[Main](layouts/samples/sample10.cs)]

執行*AddMovie.cshtml*頁面。 您會看到那里新的標題：

![顯示將加入電影標題，以動態方式建立瀏覽器索引標籤](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>更新其餘的頁面使用的版面配置

現在您可以在您的站台完成其餘的頁面，使其使用新的版面配置。 開啟*EditMovie.cshtml*和*DeleteMovie.cshtml*中開啟，並在每個進行相同變更。

新增連結至版面配置頁面的程式碼行：

[!code-csharp[Main](layouts/samples/sample11.cs)]

加入線條以設定頁面的標題：

[!code-csharp[Main](layouts/samples/sample12.cs)]

或：

[!code-csharp[Main](layouts/samples/sample13.cs)]

移除所有多餘的 HTML 標記 — 基本上，將保留在內部的位元`<body>`元素 （加上程式碼區塊頂端）。

變更`<h1>`的項目可以`<h2>`項目。

當您完成這些變更，每個測試，並請確定正確顯示，並標題正確。

## <a name="parting-thoughts-about-layout-pages"></a>版面配置頁的臨別想法

在本教學課程建立 *\_Layout.cshtml*頁面上，使用`RenderBody`方法，將合併從另一頁的內容。 這是在網頁中使用的版面配置的基本模式。

版面配置頁有我們這裡未涵蓋的其他功能。 例如，您可以巢狀版面配置頁面 — 一個版面配置頁接著可以參考另一個。 巢狀的配置很有用，如果您正在使用的站台需要不同的版面配置的各小節。 您也可以使用其他方法 (例如， `RenderSection`) 來設定名為 [配置] 頁面中的章節。

版面配置頁的組合和*.css*檔案是強大的功能。 您會看到下一個教學課程中，在 WebMatrix 中您可以建立根據*範本*，可讓您的網站中預先建立的功能。 範本可讓充分利用版面配置頁及 CSS 來建立的網站，看起來不錯，而且有類似功能表的功能。 以下是從範本，顯示使用版面配置頁和 CSS 的功能為基礎的網站 [首頁] 的螢幕擷取畫面：

![顯示標頭、 導覽區域中，內容區域、 選擇性區段和登入連結的 WebMatrix 網站範本所建立的版面配置](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>電影頁面 （若要使用的版面配置頁面更新） 的完整清單

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>完成清單 頁面新增電影的頁面 （針對版面配置更新）

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>頁面的完整清單刪除影片頁面 （針對版面配置更新）

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>頁面的完整清單編輯電影頁面 （針對版面配置更新）

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>接下來

在下一個教學課程中，您將學習如何將網站發佈到網際網路，好讓每個人都可以看到它。

## <a name="additional-resources"></a>其他資源

- [建立一致的外觀](https://go.microsoft.com/fwlink/?LinkID=202891)— 使用配置提供更多詳細資料的發行項。 它也會說明如何將值傳遞至版面配置頁的顯示或隱藏的某些內容。
- [巢狀配置頁面 Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — 的 Mike Brind 部落格如何巢狀化版面配置頁的範例。 （包括頁面下載）。

>[!div class="step-by-step"]
[上一頁](deleting-data.md)
[下一頁](publishing.md)
