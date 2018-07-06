---
uid: mvc/overview/performance/bundling-and-minification
title: 統合和縮製 |Microsoft Docs
author: Rick-Anderson
description: 統合和縮製有兩個技術您可以使用 ASP.NET 4.5 中，以改善要求載入時間。 統合和縮製可改善載入時間，由 reducin...
ms.author: aspnetcontent
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 090bb58f762302e0f58db7b8c005fe584e5ec419
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827371"
---
<a name="bundling-and-minification"></a>統合和縮製
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 統合和縮製有兩個技術您可以使用 ASP.NET 4.5 中，以改善要求載入時間。 統合和縮製可改善載入時間減少伺服器的要求數目，並減少大小的要求的資產 （例如 CSS 和 JavaScript。）


大部分的目前主要的瀏覽器恒薴菾[同時連線](http://www.browserscope.org/?category=network)每六個每個主機名稱。 這表示，而處理六個要求，以便在主機上的資產的其他要求都會排入瀏覽器。 下圖中的 IE F12 開發人員工具-網路索引標籤會顯示 [關於] 檢視的範例應用程式所需的資產的時間。

![B/分鐘](bundling-and-minification/_static/image1.png)

灰色長條圖會顯示要求已排入佇列等候六個連接限制瀏覽器的時間。 黃色列正是要求第一個位元組，也就是傳送要求並接收來自伺服器的第一個回應所花費的時間。 藍色長條會顯示從伺服器接收回應資料所花費的時間。 您可以按兩下以取得詳細的計時資訊資產。 例如下, 圖顯示載入的計時詳細資料 */Scripts/MyScripts/JavaScript6.js*檔案。

![](bundling-and-minification/_static/image2.png)

上圖所示**啟動**事件，提供因為瀏覽器的要求已排入佇列的時間限制的同時連線數目。 在此情況下，要求已排入佇列等候另一個要求完成的 46 毫秒。

## <a name="bundling"></a>統合

統合是 ASP.NET 4.5，可讓您輕鬆地結合或配套成單一檔案的多個檔案中的新功能。 您可以建立 CSS、 JavaScript 和其他組合。 較少的檔案表示較少的 HTTP 要求，可以改善第一次頁面載入效能。

下圖顯示之前，但這次使用統合和縮製啟用 About 檢視的相同時間檢視。

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>縮製

縮製會執行各種不同的程式碼最佳化，以指令碼或 css，例如移除不必要的泛空白字元和註解，以及縮短至某一字元的變數名稱。 請考慮下列的 JavaScript 函式。

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

之後縮製，函式會縮減為下列：

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

除了移除不必要的空白字元的註解，下列參數和變數名稱已重新命名 （縮短），如下所示：

| **原始** | **重新命名** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>影響統合和縮製

下表顯示個別列出的所有資產，與在範例程式中使用統合和縮製 (B/M) 之間的數個重要差異。

|  | **使用 B/分鐘** | **沒有 B/分鐘** | **變更** |
| --- | --- | --- | --- |
| **提出要求** | 9 | 34 | 256% |
| **傳送的 KB** | 3.26 | 11.92 | 266% |
| **接收的 KB** | 388.51 | 530 | 36% |
| **載入時間** | 510 MS | 780 MS | 53% |

傳送的位元組必須大幅降低透過搭售，瀏覽器會使用其套用於要求的 HTTP 標頭相當詳細資訊。 接收的位元組減少不大因為最大的檔案 (*Scripts\jquery-ui-1.8.11.min.js*並*Scripts\jquery-1.7.1.min.js*) 已縮減。 附註： 在使用的範例程式時機[Fiddler](http://www.fiddler2.com/fiddler2/)來模擬慢速網路工具。 (從 Fiddler**規則**功能表上，選取**效能**然後**模擬數據機速度**。)

## <a name="debugging-bundled-and-minified-javascript"></a>偵錯配套並縮短 JavaScript

就可以輕鬆地偵錯您的開發環境中的 JavaScript (其中[compilation 項目](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*檔案設定為`debug="true"`) 因為不會配套在 JavaScript 檔案或縮減。 您也可以偵錯，JavaScript 檔案會配套，並縮短的發行組建。 您使用 IE F12 開發人員工具，偵錯縮短組合，使用下列方法中所包含的 JavaScript 函式：

1. 選取 [**指令碼**索引標籤，然後選取**開始偵錯**] 按鈕。
2. 選取包含您想要使用 [資產] 按鈕進行偵錯的 JavaScript 函式的組合。  
    ![](bundling-and-minification/_static/image4.png)
3. 藉由選取格式化縮短的 JavaScript**組態 按鈕** ![](bundling-and-minification/_static/image5.png)，然後選取**格式 JavaScript**。
4. 在 **搜尋指令碼**t 輸入的方塊中，選取您想要偵錯函式的名稱。 在下圖中， **AddAltToImg**中所輸入**搜尋指令碼**t 的輸入的方塊。  
    ![](bundling-and-minification/_static/image6.png)

如需有關使用 F12 開發人員工具進行偵錯的詳細資訊，請參閱 MSDN 文章[使用 F12 開發人員工具，偵錯 JavaScript 錯誤](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx)。

## <a name="controlling-bundling-and-minification"></a>控制統合和縮製

啟用或停用中的偵錯屬性的值設定統合和縮製[compilation 項目](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*檔案。 在下列 XML 中，`debug`設為 true，統合和縮製已停用。

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

若要啟用統合和縮製，`debug`值為"false"。 您可以覆寫*Web.config*設定`EnableOptimizations`上的屬性`BundleTable`類別。 下列程式碼可讓統合和縮製，且會覆寫中的任何設定*Web.config*檔案。

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> 除非`EnableOptimizations`是`true`或 偵錯屬性中的[compilation 項目](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*檔案設定為`false`，檔案將不會搭配或縮減。 此外，將不會使用檔案的.min 版本，將會選取完整的偵錯版本。 `EnableOptimizations` 中的偵錯屬性會覆寫[compilation 項目](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*檔案


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>使用統合和縮製與 ASP.NET Web Form 和 Web Pages

- 針對網頁，請參閱部落格文章[新增至 Web Pages 網站的 Web 最佳化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- 對於 Web Form，請參閱部落格文章[新增統合和縮製 Web form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>使用統合和縮製與 ASP.NET MVC

在本節中，我們將建立 ASP.NET MVC 專案，以檢查統合和縮製。 首先，建立新的 ASP.NET MVC 網際網路專案，名為**MvcBM**而不需要變更任何預設值。

開啟*應用程式\_Start\BundleConfig.cs*檔案，並檢查`RegisterBundles`方法用來建立、 註冊和設定套件組合。 下列程式碼顯示的某一部分`RegisterBundles`方法。

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

上述程式碼會建立名為新的 JavaScript 配套 *~/bundles/jquery* ，其中包含所有適當 (是偵錯或縮減而非。*vsdoc*) 中的檔案*指令碼*符合萬用字元字串"~/Scripts/jquery-{version}.js"的資料夾。 ASP.NET MVC 4 中，這表示偵錯組態中，檔案*jquery 1.7.1.js*會新增至套件組合。 在 [發行] 組態中， *jquery 1.7.1.min.js*會加入。 統合 framework 例如遵循幾個常見的慣例：

- 「 FileX.min.js"和"FileX.js 」 存在時，請選取 「.min"檔案版本。
- 選取 偵錯的非".min 」 版本。
- 正在略過"-vsdoc 」 檔案 （例如 jquery-1.7.1-vsdoc.js)，這僅供 IntelliSense。

`{version}`萬用字元比對上述用來自動建立適當版本的 jQuery 中的 jQuery 組合您*指令碼*資料夾。 在此範例中，使用萬用字元會提供下列優點：

- 可讓您使用 NuGet 更新為較新的 jQuery 版本，而不變更上述統合的程式碼或在檢視頁面中的 jQuery 參考。
- 自動選取偵錯組態的完整版本和版本的 「.min"版本組建。

## <a name="using-a-cdn"></a>使用 CDN

 下列程式碼取代 CDN 的 jQuery 搭售方案的本機 jQuery 套件組合。

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

在上面的程式碼，jQuery 將要求從 CDN 版本模式和 jQuery 的偵錯版本會擷取在本機偵錯模式中時。 當使用 CDN 時，您應該在 CDN 要求失敗時，有後援機制。 下列標記片段從版面配置檔案顯示指令碼新增至要求 jQuery 應該 CDN 失敗的結尾。

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>建立配套

[配套](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)類別`Include`方法會採用字串陣列，其中每個字串都是資源的虛擬路徑。 下列程式碼中的 RegisterBundles 方法*應用程式\_Start\BundleConfig.cs*檔案會顯示如何將多個檔案新增至套件組合：

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[配套](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)類別`IncludeDirectory`方法提供來加入所有的檔案目錄 （和選擇性的所有子目錄） 中符合搜尋模式。 [配套](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)類別`IncludeDirectory`API 如下所示：

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

使用轉譯方法，檢視中所參考的套件組合 ( `Styles.Render` css 和`Scripts.Render`適用於 JavaScript)。 從下列標記*Views\Shared\\_Layout.cshtml*檔案會顯示預設的 ASP.NET 網際網路專案檢視如何參考 CSS 和 JavaScript 的套件組合。

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

請注意轉譯方法接受字串陣列，因此其中一行程式碼中，您可以加入多個套組。 您通常會想要使用轉譯方法建立所需的 HTML，以參考的資產。 您可以使用`Url`方法來產生資產的 URL，而不需要參考的資產所需的標記。 假設您想要使用新的 HTML5[非同步](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async)屬性。 下列程式碼示範如何參考使用 modernizr`Url`方法。

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>使用 「\*"萬用字元來選取檔案

中指定的虛擬路徑`Include`方法，並搜尋模式中`IncludeDirectory`方法可接受一個 「\*"做為前置詞或後的置字元到最後一個路徑區段中的萬用字元。 搜尋字串不區分大小寫。 `IncludeDirectory`方法可以選擇搜尋子目錄。

專案，請考慮下列 JavaScript 檔案：

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

下表所示，使用萬用字元套件組合加入的檔案：

| **Call** | **加入檔案或引發例外狀況** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js，ToggleDiv.js，ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | 無效的模式的例外狀況。 只允許前置詞或後置字元的萬用字元。 |
| Include("~/Scripts/Common/\*og.\*") | 無效的模式的例外狀況。 允許只有一個萬用字元。 |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | 無效的模式的例外狀況。 純的萬用字元片段不是有效的。 |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js，ToggleImg.js，ToggleLinks.js* |

明確地將每個檔案新增至組合是一般偏好透過萬用字元檔案載入的原因如下：

- 新增萬用字元預設的指令碼，以依字母順序，通常不是您想要載入它們。 CSS 和 JavaScript 檔案經常需要加入特定的 （非英文字母） 順序。 您可以藉由新增自訂來降低此風險[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)實作，但是會明確地將每個檔案較不易有錯誤。 例如，您可以在其中加入新資產的資料夾在未來可能需要您修改您[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)實作。
- 檢視特定檔案新增至目錄，使用載入的萬用字元可以包含在參考該配套的所有檢視中。 如果檢視特定指令碼新增至套件組合時，您可能會參考套件組合的其他檢視上的 JavaScript 錯誤。
- 匯入其他檔案的 CSS 檔案會導致匯入的檔案載入兩次。 比方說，下列程式碼會建立組合與大部分的載入兩次的 jQuery UI 佈景主題 CSS 檔案。 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  萬用字元選取器"\*.css 」 會在資料夾中，每個 CSS 檔案中包括*Content\themes\base\jquery.ui.all.css*檔案。 *Jquery.ui.all.css*檔匯入其他 CSS 檔案。

## <a name="bundle-caching"></a>快取項目組合

套件組合是在建立套件組合時，一年將設定 HTTP 過期標頭。 如果您瀏覽至先前檢視的頁面上，Fiddler 顯示 IE 不會進行條件式要求的組合，亦即，沒有來自 HTTP GET 要求的套件組合的 IE 並沒有從伺服器產生 HTTP 304 回應。 您可以強制使用 F5 鍵 （產生 HTTP 304 回應的每一個套件組合中） 進行條件式要求每個組合的 IE。 您可以使用，以強制完整的重新整理 ^ F5 （結果會導致每一個套件組合的 HTTP 200 回應。）

下圖顯示**快取**Fiddler 回應 窗格的索引標籤：

![fiddler 快取映像](bundling-and-minification/_static/image8.png)

要求   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 是套件組合**AllMyScripts**和包含查詢字串組**v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**。 查詢字串**v** k 也就是用於快取的唯一識別碼的值。 ASP.NET 應用程式，只要組合不會變更，將會要求**AllMyScripts**組合使用這個語彙基元。 如果套件組合中的任何檔案有所變更，asp.net WEB 最佳化架構會產生新的權杖，並保證組合的瀏覽器要求會取得最新的套件組合。

如果您執行 IE9 F12 開發人員工具，並瀏覽至先前載入的頁面，即不正確地顯示對每個組合和伺服器傳回 HTTP 304 的條件式 GET 要求。 您可以閱讀 IE9 包含判斷條件式要求的部落格文章中的問題的原因[使用 Cdn 和改善網站效能的 Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS CoffeeScript、 SCSS，Sass 統合。

統合和縮製架構提供一個機制，例如處理中繼語言[SCSS](http://sass-lang.com/)， [Sass](http://sass-lang.com/)，[較少](http://www.dotlesscss.org/)或[Coffeescript](http://coffeescript.org/)，並將例如縮製的轉換套用至產生的套件組合。 例如，若要新增[.less](http://www.dotlesscss.org/)至 MVC 4 專案的檔案：

1. 建立資料夾，以對您較少的內容。 下列範例會使用*Content\MyLess*資料夾。
2. 新增[.less](http://www.dotlesscss.org/) NuGet 套件**無點**至您的專案。  
    ![NuGet 無點安裝](bundling-and-minification/_static/image9.png)
3. 加入類別可實作[IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx)介面。 .Less 轉換，將下列程式碼新增至您的專案。

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. 建立具有較少檔案的套件組合`LessTransform`而[CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx)轉換。 將下列程式碼加入`RegisterBundles`方法中的*應用程式\_Start\BundleConfig.cs*檔案。

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. 會參考較少的套件組合的任何檢視中加入下列程式碼。

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>套件組合的考量

建立套件組合時要遵循良好的慣例是加入 「 組合 」 做套件組合名稱的前置詞。 這將導致可能[路由衝突](https://forums.asp.net/post/5012037.aspx)。

一旦您更新配套中的一個檔案時，新的權杖會產生套件組合的查詢字串參數，且完整的組合必須下載用戶端要求網頁包含組合在下一次。 在傳統標記中個別列出每個資產時，會下載變更的檔案。 經常變更的資產統合，可能不是很好的候選項目。

統合和縮製主要改善的第一個頁面要求載入時間。 一旦已要求網頁，瀏覽器快取的資產 （JavaScript、 CSS 和映像） 讓統合和縮製不提供任何效能提升時要求相同的頁面上，或在相同的網頁站台要求相同的資產。 如果您未設定 expires 標頭，在您的資產上的正確和不使用統合和縮製、 瀏覽器有效性啟發學習法會將標示為資產過時幾天後和瀏覽器將會針對每個資產需要將驗證要求。 在此情況下，統合和縮製之後第一個網頁要求，提供的效能提升。 如需詳細資訊，請參閱部落格[使用 Cdn 和改善網站效能的 Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

您可以降低每個主機名稱每六個同時連線的瀏覽器限制使用[CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。 因為 CDN 將會有不同的主機名稱，比您裝載網站，從 CDN 資產的要求將不會列入裝載環境的六個同時連線限制。 CDN 也可以提供常見的套件快取和邊緣快取的優點。

套件組合應分割所需要的頁面。 例如，預設的網際網路應用程式的 ASP.NET MVC 範本會建立 jQuery 驗證套件組合分開 jQuery。 建立的預設檢視會不有任何輸入，而且不會回傳值，因為它們不包含驗證套件組合。

`System.Web.Optimization` System.Web.Optimization.DLL 中實作命名空間。 它會利用 WebGrease 程式庫 (WebGrease.dll) 進行縮製的功能，它會使用 Antlr3.Runtime.dll。

*我可以使用 Twitter 進行快速的文章，並分享連結。我的 Twitter 控制代碼是*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>其他資源

- 影片：[統合和最佳化](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing)由[Howard Dierking](https://twitter.com/#!/howard_dierking)
- [加入 Web Pages 網站中的 Web 最佳化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- [新增統合和縮製 Web form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。
- [效能影響的統合和縮製在網頁瀏覽](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx)由[Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [使用 Cdn，以改善網站效能到期](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)由 Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [最小化 RTT （來回行程時間）](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao 是一隻
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
