---
uid: mvc/overview/performance/bundling-and-minification
title: "統合及縮製 |Microsoft 文件"
author: Rick-Anderson
description: "統合及縮製是兩項技術可用於 ASP.NET 4.5 來改善要求載入時間。 統合及縮製 reducin 藉此改善載入時間..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: e83be2446ef1e3ff1275d06d5b743fb5b9444a6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="bundling-and-minification"></a>統合及縮製
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 統合及縮製是兩項技術可用於 ASP.NET 4.5 來改善要求載入時間。 統合及縮製透過減少伺服器的要求數目以及降低要求資產 （例如 CSS 和 JavaScript。） 的大小，改進載入時間


目前的主要瀏覽器的大部分限制數目[同時連線](http://www.browserscope.org/?category=network)每為六個每個主機名稱。 這表示，而處理六個要求，以便在主機上的資產的其他要求都會排入佇列瀏覽器。 下面的影像中的 IE F12 開發人員工具-網路索引標籤會顯示 [關於] 檢視的範例應用程式所需的資產的時間。

![B/M](bundling-and-minification/_static/image1.png)

灰色長條顯示六個連接限制在等候瀏覽器的要求會排入佇列的時間。 黃色列是要求時機，第一個位元組，也就是將要求傳送，並從伺服器接收第一個回應所花費的時間。 藍色巡覽列會顯示從伺服器收到的回應資料所花費的時間。 您可以按兩下以取得詳細的計時資訊資產。 例如下, 圖顯示計時詳細資料載入*/Scripts/MyScripts/JavaScript6.js*檔案。

![](bundling-and-minification/_static/image2.png)

上述的影像顯示**啟動**事件時，哪些提供因為瀏覽器的要求已排入佇列的時間限制的同時連線數目。 在此情況下，要求已排入佇列等候另一個要求完成 46 毫秒。

## <a name="bundling"></a>結合在一起

結合在一起是 ASP.NET 4.5，可讓您輕鬆地結合或多個檔案配套成單一檔案中的新功能。 您可以建立 CSS、 JavaScript 和其他組合。 較少的檔案表示更少的 HTTP 要求，可改善第一個頁面負載效能。

下圖顯示相同的時間檢視關於檢視顯示之前，但這次與統合及縮製已啟用。

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>縮小

縮製執行各種不同的程式碼最佳化，以指令碼或 css，例如移除不必要的空白字元和註解，以及縮短成一個字元的變數名稱。 請考慮下列的 JavaScript 函式。

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

之後縮製，函式會減少所示：

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

移除註解和不必要的空白字元，除了下列參數和變數名稱已重新命名 （縮短），如下所示：

| **原始** | **重新命名** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>影響的組合和縮製

下表顯示個別列出所有資產及範例程式中使用統合及縮製 (B/M) 之間的數個重要差異。

|  | **使用 B/M** | **沒有 B/M** | **變更** |
| --- | --- | --- | --- |
| **檔案要求** | 9 | 34 | 256% |
| **傳送的 KB** | 3.26 | 11.92 | 266% |
| **接收的 KB** | 388.51 | 530 | 36% |
| **載入時間** | 510 MS | 780 MS | 53% |

傳送的位元組有大幅降低與結合在一起的瀏覽器相當詳細資訊，以套用要求的 HTTP 標頭。 接收的位元組減少不一樣大因為最大檔案 (*Scripts\jquery-ui-1.8.11.min.js*和*Scripts\jquery-1.7.1.min.js*) 已經縮短。 注意： 上使用的範例程式時機[Fiddler](http://www.fiddler2.com/fiddler2/)工具來模擬慢速網路。 (從 Fiddler**規則**功能表上，選取**效能**然後**模擬數據機速度**。)

## <a name="debugging-bundled-and-minified-javascript"></a>偵錯配套並縮短 JavaScript

所以可以輕鬆地偵錯您的 JavaScript，在開發環境中 (其中[compilation 元素](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx)中*Web.config*檔案設定為`debug="true"`) 因為不會集結 JavaScript 檔案或縮短。 您也可以偵錯的 JavaScript 檔案包裝在一起，而縮短的發行組建。 您使用 IE F12 開發人員工具，偵錯使用下列方式縮短組合中所包含的 JavaScript 函式：

1. 選取**指令碼**索引標籤，然後選取 [**開始偵錯**] 按鈕。
2. 選取包含您想要使用 [資產] 按鈕進行偵錯 JavaScript 函式的組合。  
    ![](bundling-and-minification/_static/image4.png)
3. 藉由選取格式化縮短的 JavaScript**組態按鈕** ![](bundling-and-minification/_static/image5.png)，然後選取**格式 JavaScript**。
4. 在**搜尋指令碼**t 輸入的方塊中，選取您要偵錯函式的名稱。 在下圖**AddAltToImg**輸入**搜尋指令碼**t 輸入的方塊。  
    ![](bundling-and-minification/_static/image6.png)

如需有關如何使用 F12 開發人員工具進行偵錯的詳細資訊，請參閱 MSDN 文章：[使用 F12 開發人員工具，偵錯 JavaScript 錯誤](https://msdn.microsoft.com/en-us/library/ie/gg699336(v=vs.85).aspx)。

## <a name="controlling-bundling-and-minification"></a>控制統合及縮製

統合及縮製已啟用或停用偵錯屬性中的值設定[compilation 元素](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx)中*Web.config*檔案。 在下列 XML 中，`debug`設定為 true，組合和縮製已停用。

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

若要啟用統合及縮製，`debug`值為"false"。 您可以覆寫*Web.config*設定`EnableOptimizations`屬性`BundleTable`類別。 下列程式碼可讓統合及縮製，並覆寫中的任何設定*Web.config*檔案。

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> 除非`EnableOptimizations`是`true`或中的偵錯屬性[compilation 元素](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx)中*Web.config*檔案設定為`false`，檔案將不會搭配或縮短。 此外，將不會使用檔案的.min 版本，將選取完整的偵錯版本。 `EnableOptimizations`覆寫中的偵錯屬性[compilation 元素](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx)中*Web.config*檔案


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>使用組合和縮製與 ASP.NET Web Form 和 Web 網頁

- 網頁，請參閱部落格文章： [Web Pages 站台中新增 Web 最佳化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- Web Form，請參閱部落格文章：[加入統合及縮製到 Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>使用組合和縮製搭配 ASP.NET MVC

在本節中，我們將建立 ASP.NET MVC 專案，以檢查組合和縮製。 首先，建立新的 ASP.NET MVC 網際網路專案，名為**MvcBM**而不需要變更任何預設值。

開啟*應用程式\_Start\BundleConfig.cs*檔案，並檢查`RegisterBundles`方法用來建立、 註冊和設定組合。 下列程式碼顯示的某一部分`RegisterBundles`方法。

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

上述程式碼會建立新的 JavaScript 組合，名為*~/bundles/jquery* ，其中包含所有適當 (是偵錯或縮短但不是。*vsdoc*) 中的檔案*指令碼*符合萬用字元字串"~/Scripts/jquery-{版本}.js"的資料夾。 ASP.NET MVC 4，這表示偵錯組態，將檔案與*jquery 1.7.1.js*將會新增至組合。 在 [發行] 組態*jquery 1.7.1.min.js*將加入。 將架構例如遵循幾個常見的慣例：

- 存在 「 FileX.min.js"和"FileX.js 」 時，請選取一版的 「.min"檔案。
- 選取偵錯的非".min 」 版本。
- 正在略過 」-vsdoc 」 檔案 （例如 jquery-1.7.1-vsdoc.js)，這只由 IntelliSense。

`{version}`萬用字元比對上述用來自動建立與 jQuery 中的適當版本的 jQuery 組合您*指令碼*資料夾。 此範例中，使用萬用字元，提供下列優點：

- 可讓您使用 NuGet 而不需要變更上述將程式碼或檢視頁面中的 jQuery 參考更新為較新的 jQuery 版本。
- 自動選取偵錯組態的完整版本和版本的 「.min 」 版本組建。

## <a name="using-a-cdn"></a>使用 CDN

 下列程式碼取代本機 jQuery 組合使用 CDN jQuery 套件組合。

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

上述程式碼，jQuery 將要求從 CDN 時在發行模式和 jQuery 的偵錯版本將會提取在本機偵錯模式中。 當使用 CDN 時，您應該在 CDN 要求失敗時，有後援機制。 下列標記片段從配置檔案顯示指令碼新增至要求 jQuery 應該 CDN 失敗的結尾。

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>建立套件組合

[配套](https://msdn.microsoft.com/en-us/library/system.web.optimization.bundle(v=VS.110).aspx)類別`Include`方法會採用字串陣列，其中每個字串都是資源的虛擬路徑。 下列程式碼中的 RegisterBundles 方法從*應用程式\_Start\BundleConfig.cs*檔會示範如何在多個檔案會新增至組合：

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[配套](https://msdn.microsoft.com/en-us/library/system.web.optimization.bundle(v=VS.110).aspx)類別`IncludeDirectory`方法提供來加入所有的檔案的目錄 （以及選擇性地所有子目錄） 中符合搜尋模式。 [配套](https://msdn.microsoft.com/en-us/library/system.web.optimization.bundle(v=VS.110).aspx)類別`IncludeDirectory`API 如下所示：

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

使用轉譯方法，檢視中所參考的組合 ( `Styles.Render` css 和`Scripts.Render`javascript)。 從下列標記*_layout.cshtml\\_Layout.cshtml*檔案會顯示預設的 ASP.NET 網際網路專案檢視如何參考 CSS 和 JavaScript 的組合。

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

請注意轉譯方法會採用字串陣列，以便在一行程式碼，您可以加入多種組合。 您通常會想要使用轉譯方法會建立必要的 HTML，以便參考資產。 您可以使用`Url`方法來產生資產的 URL，而不需要參考資產的標記。 假設您想要使用新的 HTML5[非同步](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async)屬性。 下列程式碼示範如何參考使用 modernizr`Url`方法。

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>使用 「\*"萬用字元來選取檔案

中指定的虛擬路徑`Include`方法，並搜尋模式中`IncludeDirectory`方法可以接受一"\*"做為前置詞或後置詞的最後一個路徑區段中的萬用字元。 搜尋字串是不區分大小寫。 `IncludeDirectory`方法都有的搜尋子目錄選項。

專案，請考慮下列 JavaScript 檔案：

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

下表所示，使用萬用字元組合加入的檔案：

| **Call** | **加入檔案或引發例外狀況** |
| --- | --- |
| 包含 (「 ~/Scripts/Common/\*.js") | *AddAltToImg.js，ToggleDiv.js，ToggleImg.js* |
| 包含 (「 ~/Scripts/Common/T\*.js") | 無效的模式例外狀況。 前置詞或後置詞只允許萬用字元。 |
| 包含 (「 ~/Scripts/Common/\*og。\*") | 無效的模式例外狀況。 允許只能有一個萬用字元。 |
| 「 包含 (「 ~/Scripts/Common/T\*") | *ToggleDiv.js ToggleImg.js* |
| 「 包含 (「 ~/Scripts/Common/\*") | 無效的模式例外狀況。 純的萬用字元片段無效。 |
| IncludeDirectory (「 ~/Scripts/Common"、"T\*") | *ToggleDiv.js ToggleImg.js* |
| IncludeDirectory (「 ~/Scripts/Common"、"T\*"，則為 true) | *ToggleDiv.js，ToggleImg.js，ToggleLinks.js* |

明確地將每個檔案加入至組合是通常慣用透過萬用字元載入的檔案，原因如下：

- 加入指令碼萬用字元預設依字母順序，通常不是您想要載入它們。 若要新增特定的 （非英文字母） 順序經常需要 CSS 和 JavaScript 檔案。 您可以新增自訂來降低此風險[IBundleOrderer](https://msdn.microsoft.com/en-us/library/system.web.optimization.ibundleorderer(VS.110).aspx)實作，但是會明確地將每個檔案較不易有錯誤。 比方說，您可以在其中加入新資產的資料夾在未來可能需要修改您[IBundleOrderer](https://msdn.microsoft.com/en-us/library/system.web.optimization.ibundleorderer(VS.110).aspx)實作。
- 檢視特定檔案新增至目錄，使用載入的萬用字元可以包含在所有檢視中參考該配套。 如果檢視特定指令碼新增至組合，您可能會參考組合之其他檢視上的 JavaScript 錯誤。
- CSS 檔案匯入其他檔案，會導致匯入的檔案載入兩次。 例如，下列程式碼建立組合與大部分的載入兩次的 jQuery UI 佈景主題 CSS 檔。 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

 萬用字元選取器 」\*.css 」 會在資料夾中，每個 CSS 檔案中包括*Content\themes\base\jquery.ui.all.css*檔案。 *Jquery.ui.all.css*檔匯入其他 CSS 檔案。

## <a name="bundle-caching"></a>快取中組合

組合設定組合建立時從 HTTP 到期標頭一年。 如果您瀏覽至先前檢視過的頁面上，Fiddler 顯示 IE 不會進行條件式要求的組合，亦即沒有來自 HTTP GET 要求組合的 IE 和來自伺服器的 HTTP 304 回應。 您可以強制 IE （導致每個組合的 HTTP 304 回應） F5 鍵以進行條件式要求每個組合。 可以使用強制執行完整的重新整理 ^ F5 （導致每個組合的 HTTP 200 回應）。

下圖顯示**快取**Fiddler 回應窗格 索引標籤：

![fiddler 快取映像](bundling-and-minification/_static/image8.png)

要求   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 組合是**AllMyScripts**和包含查詢字串組**v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**。 查詢字串**v**語彙基元也就是用來快取的唯一識別碼的值。 ASP.NET 應用程式，只要組合不會變更，將要求**AllMyScripts**組合使用這個語彙基元。 如果變更組合中的任何檔案，asp.net WEB 最佳化架構會產生新的權杖，以及確保組合的瀏覽器要求會取得最新的套件組合。

如果您執行 IE9 F12 開發人員工具，並瀏覽至先前載入的頁面，即不正確地顯示對每個組合和伺服器傳回 HTTP 304 的條件式 GET 要求。 您可以閱讀 IE9 為什麼有問題，判斷條件式要求的部落格文章中[使用 Cdn 和改善網站效能的到期日](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

## <a name="less-coffeescript-scss-sass-bundling"></a>較少，CoffeeScript、 SCSS，sass 此類結合在一起。

統合及縮製架構提供一個機制，例如處理中繼語言[SCSS](http://sass-lang.com/)， [Sass](http://sass-lang.com/)，[較少](http://www.dotlesscss.org/)或[Coffeescript](http://coffeescript.org/)，並將例如縮製的轉換套用至產生的套件組合。 例如，若要新增[.less](http://www.dotlesscss.org/)至 MVC 4 專案的檔案：

1. 建立適用於您較少的內容資料夾。 下列範例會使用*Content\MyLess*資料夾。
2. 新增[.less](http://www.dotlesscss.org/) NuGet 封裝**無點**至您的專案。  
    ![NuGet 無點安裝](bundling-and-minification/_static/image9.png)
3. 加入的類別，實作[IBundleTransform](https://msdn.microsoft.com/en-us/library/system.web.optimization.ibundletransform(VS.110).aspx)介面。 .Less 轉換，請將下列程式碼加入您的專案。

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. 建立具有較少檔案的組合`LessTransform`和[CssMinify](https://msdn.microsoft.com/en-us/library/system.web.optimization.cssminify(VS.110).aspx)轉換。 將下列程式碼加入`RegisterBundles`方法中的*應用程式\_Start\BundleConfig.cs*檔案。

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. 將下列程式碼加入至參考較少的資源存放區的任何檢視。

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>配套的考量

建立套件組合時所應遵循的良好慣例是納入 「 組合 」 中的資源存放區名稱的前置詞。 這會避免可能[路由衝突](https://forums.asp.net/post/5012037.aspx)。

一旦您更新配套中的一個檔案時，套件組合的查詢字串參數產生新的權杖和完整的組合必須下載用戶端要求網頁包含組合在下一次。 在傳統標記中個別列出每個資產時，會下載已變更的檔案。 經常變更的資產可能不是理想的對象的結合在一起。

統合及縮製主要改善第一個頁面要求載入時間。 一旦要求的網頁，瀏覽器快取資產 （JavaScript、 CSS 和圖像） 因此統合及縮製將不提供任何提升效能，當要求相同的頁面上，或在相同的網頁站台要求相同的資產。 如果您不需要設定到期標頭，在您的資產上正確和不使用統合及縮製、 瀏覽器有效性啟發學習法會將標示為資產過時幾天之後和瀏覽器將會需要為每個資產的驗證要求。 在此情況下，統合及縮製的第一個頁面要求之後，提供的效能提升。 如需詳細資訊，請參閱部落格[使用 Cdn 和改善網站效能的到期日](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

每個每個主機名稱的六個同時連線的瀏覽器限制可減輕使用[CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。 CDN 會有不同的主機名稱與您裝載站台，因為 cdn 資產要求將不會列入裝載環境的六個同時連線限制。 CDN 時，也可以提供一般封裝快取和邊緣快取的優點。

組合應該由需要它們的頁面分割。 例如的預設網際網路應用程式的 ASP.NET MVC 範本會建立 jQuery 驗證配套分開 jQuery。 建立的預設檢視有沒有輸入，並不會回傳值，因為它們不包含驗證套件組合。

`System.Web.Optimization` System.Web.Optimization.DLL 中實作的命名空間。 它會利用 WebGrease 程式庫 (WebGrease.dll) 縮製功能，接著使用 Antlr3.Runtime.dll。

*使用 Twitter 進行快速文章及共用的連結。我 Twitter 控點是*:[@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>其他資源

- 影片：[統合及最佳化](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing)由[Howard Dierking](https://twitter.com/#!/howard_dierking)
- [加入 Web Pages 站台中的 Web 最佳化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- [新增組合和縮製到 Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。
- [效能影響的組合和縮製的網頁瀏覽](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx)由[Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen)[@frystyk](https://twitter.com/frystyk)
- [使用 Cdn，而且已過期來改善網站效能](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)由 Rick anderson 發表[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [最小化 RTT （來回行程時間）](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao 是一隻
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
