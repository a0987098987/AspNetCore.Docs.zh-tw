---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 簡介 ASP.NET Web Pages-程式設計基本概念 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程可提供您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。 您將學到什麼： 您的提取要求所使用的基本 'Razor' 語法...
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: ec1c055d1b3f6ca5c6374a18840c2595bb368e0e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021530"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>ASP.NET Web Pages 簡介-程式設計基本概念
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程可提供您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。
> 
> 您將學到什麼：
> 
> - 您用於 ASP.NET Web Pages 中的程式設計基本 「 Razor 」 語法。
> - 一些基本 C# 中，也就是您將使用的程式設計語言。
> - 網頁的某些基本程式設計概念。
> - 如何安裝封裝 （包含預先建置的程式碼的元件） 以使用與您的網站。
> - 如何使用*協助程式*來執行常見程式設計工作。
>   
> 
> 功能/技術討論：
> 
> - NuGet 和套件管理員。
> - `Gravatar`協助程式。


本教學課程是向您介紹程式設計的語法，您將使用 ASP.NET Web Pages 中的主要活動。 您將了解*Razor 語法*和 C# 中撰寫的程式碼程式設計語言。 您在上一個教學課程中，有一窺這種語法在本教學課程中，我們將說明詳細的語法。

我們保證，本教學課程包含程式設計，您會看到在單一的教學課程中，以及它是唯一的教學課程，是最*只*關於程式設計。 在此集合中其餘的教學課程，您將實際建立有趣的事情頁面。

您也將了解*協助程式*。 協助程式是一種元件 — 封裝最多段程式碼 — 您可以加入至頁面。 協助程式會執行，否則可能會很繁瑣或是以手動方式執行複雜的工作。

## <a name="creating-a-page-to-play-with-razor"></a>建立使用 Razor 頁面

在本節中您將播放有點 Razor 的因此您可以了解的基本語法。

如果尚未執行，請啟動 WebMatrix。 您將使用您在上一個教學課程中建立的網站 ([取得開始使用 Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578))。 若要重新開啟它，請按一下**My Sites** ，然後選擇**WebPageMovies**:

![WebMatrix 啟動畫面，顯示反白顯示了我的網站與開啟的站台選項](intro-to-web-pages-programming/_static/image1.png)

選取 **檔案**工作區。

在 功能區中，按一下**新增**建立頁面。 選取  **CSHTML**並將新頁面命名*TestRazor.cshtml*。

按一下 [確定 **Deploying Office Solutions**]。

將以下內容複製到檔案中，完全取代項目已經有。

> [!NOTE]
> 當您到頁面上，複製程式碼或標記的範例時，縮排和對齊方式可能無法與本教學課程相同。 縮排和對齊方式不會影響程式碼的執行方式，不過。


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>檢查範例頁面

您所看到的大部分是一般的 HTML。 不過，在頂端沒有此程式碼區塊：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

請注意下列事項，關於此程式碼區塊：

- @ 字元會告知 ASP.NET，接下來是 Razor 程式碼，而不是 HTML。 ASP.NET 會將所有項目之後 @ 字元，直到它再次執行成一些 HTML 程式碼。 (在此情況下，這&lt;！DOCTYPE&gt;項目。
- 大括號 （{和}） 括住的 Razor 程式碼區塊，如果程式碼有多個列。 大括號會告知 ASP.NET，該區塊的程式碼開頭與結尾的位置。
- / / 字元標示註解，也就是不會執行的程式碼的一部分。
- 每個陳述式必須以分號 （;） 結尾。 （未註解，雖然。）
- 您可以儲存中的值*變數*，您建立 (*宣告*) 與關鍵字 var。 當您建立變數時，您為它命名，其中可以包含字母、 數字和底線 (\_)。 變數名稱不能以數字開頭，而且無法使用程式設計的關鍵字 （例如 var) 的名稱。
- 以引號括住字元字串 （例如 「 ASP.NET 」 和 「 網頁 」）。 （它們必須是雙引號）。數字不在引號中。
- 引號之外的空白區域並不重要。 換行大部分不重要;例外情況是您無法跨行分割引號的字串。 縮排和對齊方式沒有影響。

不是明顯來自此範例中的項目是所有的程式碼會區分大小寫。 這表示變數的總和是不同的變數數目多於可能名為總和的變數數目。 同樣地，var 是關鍵字，但不是 Var。

### <a name="objects-and-properties-and-methods"></a>物件與屬性和方法

然後是 DateTime.Now 的運算式。 簡單地說，是日期時間*物件*。 物件是您可以使用程式設計的功能 — 頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄等等。物件具有一或多個*屬性*描述它們的特性。 文字 方塊物件都有文字屬性 （還有其他）、 request 物件具有 Url 屬性 （以及其他）、 電子郵件訊息有 From 屬性和 To 屬性，依此類推。 物件也有*方法*，是他們可以執行 「 動詞 」。 您會使用物件很多。

您可以看到範例中，日期時間是可讓您程式的日期和時間的物件。 它具有名為現在會傳回目前的日期和時間的屬性。

### <a name="using-code-to-render-markup-in-the-page"></a>使用程式碼來呈現頁面中的標記

在頁面的主體，請注意下列各項：

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

同樣地，@ 字元會告知 ASP.NET，以下是程式碼，不是 HTML。 在標記中您可以加上 @ 後面的程式碼運算式，而 ASP.NET 會在該點轉譯該運算式右邊的值。 在範例中，@a將會呈現任何內容的值是名為的變數，@product呈現無論是在變數的具名產品中，並依此類推。

您可以使用不到變數，不過。 在少數情況下在這裡，@ 字元前面的運算式：

- @(\*b） 呈現的變數中的產品和 b。 (\*運算子表示乘法。)
- @(技術 +""+ 產品) 之後將它們串連並之間加入空格呈現變數技術和產品中的值。 將字串串連運算子 （+） 是與運算子相同個數字相加。 ASP.NET 通常會告訴您是否正在與數字或字串，並進行正確的作業，使用 + 運算子。
- @Request.Url 呈現要求物件的 Url 屬性。 要求物件包含瀏覽器中，從目前要求的相關資訊，而當然 Url 屬性包含目前要求的 URL。

此範例也可顯示您可以執行不同的方式運作。 您可以執行程式碼區塊頂端的計算，將結果放入變數中，然後轉譯在標記中的變數。 或者，您可以執行在標記中的運算式右邊的計算結果。 您使用的方法取決於您正在做什麼，以及某種程度來說，在您自己的喜好設定。

### <a name="seeing-the-code-in-action"></a>查看動作中的程式碼

以滑鼠右鍵按一下檔案的名稱，然後選擇 **在瀏覽器中啟動**。 您會看到所有的值和在頁面中解析運算式的瀏覽器頁面。

![在瀏覽器中執行的 'TestRazor' 頁面](intro-to-web-pages-programming/_static/image2.png)

看看瀏覽器中的來源。

![在瀏覽器中測試 Razor 頁面原始碼](intro-to-web-pages-programming/_static/image3.png)

如您預期您在上一個教學課程中的體驗，Razor 程式碼都是在的頁面。 您會看到所有會實際顯示的值。 當您執行頁面時，您實際上正在要求 WebMatrix 內建的 web 伺服器。 當收到要求時，ASP.NET 會解析所有的值和運算式，並將其值轉譯成網頁。 然後，將頁面傳送至瀏覽器。

> [!TIP] 
> 
> **Razor 和 C#**
> 
> 到目前為止我們所說的內容，您正在使用 Razor 語法。 為 true，但它不是完成的劇本。 實際的程式設計語言，您使用稱為*C#*。 C# 年前由 Microsoft 所建立而且已經成為主要的程式設計語言，用於建立 Windows 應用程式的其中一個。 您已了解有關如何為變數命名，以及如何建立陳述式等的所有規則都是實際的 C# 語言的所有規則。
> 
> Razor 更具體來說是指在小型組的慣例，您將此程式碼內嵌到頁面。 例如，使用標記頁面中的程式碼和使用的慣例 @ {} 內嵌程式碼區塊是頁面的 Razor 層面。 協助程式也會被視為 Razor 的一部分。 Razor 語法用在更多地方比只是 ASP.NET Web Pages 中。 （例如，它使用 ASP.NET MVC 檢視表中）。
> 
> 我們之所以提到這，因為如果您尋找程式 ASP.NET Web Pages 」 的相關資訊，您會發現許多 Razor 的參考。 不過，這些參考大量不適用於什麼是這麼做，因此可能會造成混淆。 而且事實上，許多程式設計問題的真正想要使用 C#，或使用 ASP.NET 的相關。 因此如果您特別尋找 Razor 的相關資訊，您可能無法找到所需的解答。


## <a name="adding-some-conditional-logic"></a>新增一些條件式邏輯

在網頁中使用程式碼的相關最棒的功能之一是，您可以變更會發生什麼事根據各種條件。 在本教學課程的這個部分，您會試用一些方法，若要變更頁面中顯示的內容。

此範例會簡單而且較自然，讓我們可以專注於條件式邏輯。 您將建立的網頁將會執行這項操作：

- 取決於它是否為第一次顯示網頁時，或是否已按下送出頁面上的按鈕，請在頁面上顯示不同的文字。 這會是第一項條件式測試。
- 為某個值傳入查詢字串的 URL (http://...?show=true) 時，才會顯示訊息。 這會是第二個條件測試。

在 WebMatrix 中，建立網頁並將它命名*TestRazorPart2.cshtml*。 (在功能區中，按一下**的新**，選擇**CSHTML**檔案，命名，然後按一下 **確定**。)

以下列取代該網頁的內容：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

在頂端的程式碼區塊會初始化名為一些文字訊息的變數。 在頁面的主體中，訊息變數的內容會顯示在&lt;p&gt;項目。 也包含標記&lt;輸入&gt;元素，來建立**送出** 按鈕。

執行頁面，以查看它如何運作。 現在，它基本上是靜態的頁面上，即使您按**送出** 按鈕。

返回至 WebMatrix。 在程式碼區塊中，新增下列醒目提示的程式碼*之後*初始化訊息的那一行：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If {} 區塊

您剛才加入的 if 條件。 在程式碼，如果條件有如下的結構：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

要測試的條件是括號括住。 它必須是值或傳回 true 或 false 的運算式。 如果條件為 true，ASP.NET 會執行陳述式或陳述式的括號內。 (這些都是*再*屬於*if-then*邏輯。)如果條件為 false，則會略過的程式碼區塊。

以下是一些您可以在 if 測試的條件的範例陳述式：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

您可以使用，以測試針對值或運算式的變數<em>邏輯運算子</em>或是<em>比較運算子</em>： 等於 （= =）、 大於 (&gt;)、 小於 (&lt;)，大於或等於 (&gt;=)，且小於或等於 (&lt;=)。 ！ = 運算子表示不等於 — 比方說，如果 (！ = 0) 表示<em>如果</em> <em>不等於 0</em>。

> [!NOTE]
> 請確定您會注意到，等於 （= =） 比較運算子不相同 =。 = 運算子只能用來將值指派 (var = 2)。 如果您混這些運算子時，您可能會收到錯誤，否則您會得到一些奇怪的結果。


若要測試的項目是否為 true，完整的語法是 if(IsDone == true)。 但是，您也可以使用捷徑 if(IsDone)。 如果沒有任何比較運算子，ASP.NET 會假設您要測試為 true。

！ 運算子本身表示邏輯 NOT。 例如，條件 if （！IsPost) 方法*IsPost 是否不為 true*。

您可以使用邏輯 AND 結合條件 (&amp; &amp;運算子) 或邏輯 OR (| | 運算子)。 If 的最後一個上述的範例方法中的條件，例如*如果 FileProcessingIsDone 未設定為 true AND displayMessage 設為 false*。

### <a name="the-else-block"></a>在 else 區塊

最後一有關 if 區塊： 如果區塊後面可以接著 else 區塊。 適合其他區塊是您必須在條件為 false 時，執行不同的程式碼。 以下是簡單的範例：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

在稍後的教學課程中，使用其他區塊會很實用的這一系列中，您會看到一些範例。

### <a name="testing-whether-the-request-is-a-submit-post"></a>測試這個要求提交 （文章）

還有更多，但讓我們回到範例中，有條件 if(IsPost) {...}。 IsPost 是實際目前頁面的屬性。 第一次要求頁面時，IsPost 會傳回 false。 不過，如果您按一下按鈕或否則送出頁面 — 也就是將其張貼 — IsPost，則傳回 true。 因此 IsPost 可讓您判斷您是否正在處理送出表單。 （根據 HTTP 指令動詞，如果要求是 GET 作業，IsPost 傳回 false。 如果要求是 POST 作業，IsPost 會傳回 true。）在稍後的教學課程中您將使用輸入表單，其中這項測試變得特別實用。

執行網頁。 因為這是第一次要求您頁面上，看到 「 這是您要求的頁面第一次 」。 該字串會是您初始化訊息變數的值。 If(IsPost) 測試，但讓置於 if 的程式碼區塊傳回 false 目前，不會執行。

按一下 [**送出**] 按鈕。 要求頁面時一次。 為之前，訊息變數設定為 「 這是第一次...」。 但這次測試 if(IsPost) 傳回 true，因此 if 內的程式碼區塊執行。 程式碼會變更為不同的值，也就是要呈現的標記中的訊息變數的值。

現在將 if 條件在標記中的。 下面&lt;p&gt;包含的項目**送出**按鈕，加入下列標記：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

您要新增標記內的程式碼，因此您必須開始@。 If 就類似於您新增稍早的程式碼區塊中的測試。 在大括弧，不過，您要加入一般 HTML — 至少，這是一般直到到達@DateTime.Now。 這是另一個少量 Razor 程式碼，因此一次您必須在其前面加上 @。

此處的重點是，如果您可以新增條件中的程式碼區塊的上方，並在標記中。 如果您使用 [if] 頁面上，在區塊內的行主體中的條件可以是標記或程式碼。 在此情況下，以及每當您混合使用標記和程式碼，則為 true，因為您必須使用將它清除給 ASP.NET 程式碼的所在。

執行頁面，然後按一下**送出**。 此時不只顯示不同的訊息時送出之後 （「 現在您已上傳..."），但您會看到新的訊息，列出的日期和時間。

![提交 'Test Razor 2' 的網頁瀏覽器中執行之後所顯示的時間戳記](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>測試值的查詢字串

一項更多測試。 此時，您將新增 if 測試一個值的區塊，名為 「 可能會傳遞查詢字串中的顯示。 (如下所示： `http://localhost:43097/TestRazorPart2.cshtml?show=true`)，以便顯示您已顯示，您要變更頁面 （「 這是第一次...」 等） 只有當顯示的值為 true，才會顯示。

在底部 （但內部） 程式碼區塊頂端的頁面上，新增下列內容：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

完整的程式碼區塊現在看起來與下列範例。 （請記住，當您將程式碼複製到您的頁面時，縮排看起來可能不同。 不過，不會影響程式碼的執行方式）。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

新的程式碼區塊中初始化變數，名為 showMessage 為 false。 然後再執行 if 測試，以尋找查詢字串中的值。 當您第一次要求頁面時，它會有如下的 URL:

`http://localhost:43097/TestRazorPart2.cshtml`

程式碼會判斷 URL 是否包含名為查詢字串，例如 URL 的此版本中顯示的變數：

`http://localhost:43097/TestRazorPart2.cshtml`？ 顯示 = true

測試本身會查看要求物件的查詢字串屬性。 如果查詢字串包含項目已命名的顯示，而且該項目的設定為 true，if 區塊執行，並將 showMessage 變數設定為 true。

如您所見，沒有這裡的技巧。 如其名，則查詢字串會是字串。 不過，您可以只測試 true 和 false 如果您要測試的值是布林 (true/false) 值。 您可以測試查詢字串中的顯示變數的值之前，您必須將它轉換成布林值。 這是 AsBool 方法的功能 — 它會接受字串做為輸入，並將它轉換成布林值。 很明顯地，如果字串為"true"，AsBool 方法會將該值轉換為 true。 如果字串的值可以是任何其他項目，AsBool 會傳回 false。

> [!TIP] 
> 
> **資料類型和 as （） 方法**
> 
> 我們只能說了到目前為止，當您建立變數時，您使用關鍵字 var。 這不是整個本文中，不過。 若要操作的值 — 新增數字，或串連字串，或比較日期，或測試的 true/false-C# 必須使用適當的內部表示法的值。 C# 可以*通常*找出該表示應該為何 (也就是什麼*型別*資料) 根據您所做的值。 不過它無法這麼做。 如果沒有，您必須藉由明確指出 C# 應該代表資料的方式來助一臂之力。 AsBool 方法這麼 — 它會告訴 C#"true"或"false"的字串值，應該視為布林值。 類似的方法可用來做為其他型別，表示字串，例如 AsInt （視為整數）、 AsDateTime （視為日期/時間）、 AsFloat （視為浮點數），依此類推。 當您使用這些作為 （） 方法，如果要求 C# 無法表示的字串值時，您會看到錯誤。


在頁面標記中，移除或註解此項目 （在此它會顯示標成註解）：

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

權限，讓您移除或標記為註解文字，新增下列內容：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

如果測試顯示，則為 true，showMessage 變數是否呈現&lt;p&gt;與訊息變數的值的項目。

### <a name="summary-of-your-conditional-logic"></a>您的條件式邏輯的摘要

如果您不清楚什麼您剛才所完成的此為摘要。

- 訊息變數會初始化為預設的字串 ("This is 第一次...")。
- 如果頁面要求 (post) 提交的結果，訊息的值會變成 「 現在您已提交...」
- ShowMessage 變數會初始化為 false。
- 如果查詢字串包含？ 顯示 = showMessage 變數會設定為 true，則為 true。
- 在標記中，如果 showMessage 成立&lt;p&gt;項目會呈現，顯示訊息的值。 （如果 showMessage 為 false，不會呈現在該點的標記中。）
- 在標記中，如果要求是文章時， &lt;p&gt;項目會呈現可顯示的日期和時間。

執行網頁。 沒有任何訊息，因為 showMessage 是 false，因此在標記中 if(showMessage) 測試會傳回 false。

按一下 [提交]。 您會看到日期和時間，但仍然沒有任何訊息。

在瀏覽器中移至 URL 方塊中，並將下列新增至 URL 的結尾:？ 顯示 = true，然後按 Enter。

![在瀏覽器中顯示查詢字串 'Test Razor 2' 的頁面](intro-to-web-pages-programming/_static/image5.png)

頁面會再次顯示。 （因為您變更 URL，這是新要求，而不送出）。按一下 **送出**一次。 因為日期和時間時，會一次，顯示訊息。

![查詢字串時，送出後 'Test Razor 2' 的頁面](intro-to-web-pages-programming/_static/image6.png)

在 URL 中，變更嗎？ 顯示 true =？ 顯示 = false，然後按 Enter 鍵。 提交頁面一次。 頁面會回到您的啟動方式 — 任何訊息。

如先前所述，此範例中的邏輯是有點自然。 不過，如果要出現在許多頁面，而且將會需要一或多個您已了解的形式這裡。

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>安裝協助程式 （顯示 Gravatar 影像）

使用者通常會想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。 範例： 顯示資料的圖表將 Facebook [讚] 按鈕放在頁面的從您的網站; 傳送電子郵件裁剪或調整大小的影像;為您的網站使用 PayPal。 若要讓您輕鬆地執行這類作業，ASP.NET Web Pages 可讓您使用*協助程式*。 協助程式是您安裝站台，並讓您執行一般工作所使用的 Razor 程式碼只需要幾行的元件。

ASP.NET Web Pages 有幾個內建的協助程式。 不過，許多協助程式可使用 NuGet 套件管理員提供的封裝 （增益集） 中。 NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。

在這個部分的教學課程中，您將安裝的協助程式可讓您顯示 Gravatar （「 全域可辨識的顯示圖片 」） 映像。 您將了解兩件事。 其中一個是如何尋找及安裝協助程式。 此外，您也將了解如何協助程式輕鬆地進行否則您必須利用許多您必須自行撰寫的程式碼來完成。

您可以註冊 Gravatar 網站在您自己 Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/)，但不需要建立 Gravatar 帳戶來執行本教學課程的這個部分。

在 WebMatrix 中，按一下**NuGet**  按鈕。

![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](intro-to-web-pages-programming/_static/image7.png)

這會啟動 NuGet 套件管理員，並顯示可用的套件。 (不是所有的封裝是協助程式，一些將功能新增至本身的 WebMatrix，一些其他範本，並依此類推。)您可能會收到有關版本不相容的錯誤訊息。 您可以忽略此錯誤訊息，依序按一下**確定**和繼續進行本教學課程。

![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](intro-to-web-pages-programming/_static/image8.png)

在 [搜尋] 方塊中，輸入 「 asp.net 協助程式 」。 NuGet 會顯示符合搜尋條件的封裝。

![在 WebMatrix 中顯示套件的 NuGet 資源庫](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Helpers Library 包含程式碼，以簡化許多常見的工作，包括使用 Gravatar 影像。 選取  **ASP.NET Web Helpers Library**封裝，然後按一下**安裝**啟動安裝程式。 選取 **是**時詢問您是否要安裝套件，並接受條款才能完成安裝。

就這麼容易！ NuGet 會下載並安裝所有項目，包括任何可能需要的其他元件 (*相依性*)。

如果基於某些原因，您必須解除安裝的協助程式，此程序是非常類似。 按一下  **NuGet**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。

## <a name="using-a-helper-in-a-page"></a>在網頁中使用協助程式

現在您可以使用您剛才安裝的協助程式。 加入網頁中的協助程式的程序是類似的大多數協助程式。

在 WebMatrix 中，建立網頁並將它命名*GravatarTest.cshml*。 （您要建立特殊的頁面，即可測試協助程式，但您可以使用協助程式在您的網站中的任何分頁）。

內部&lt;主體&gt;項目，新增&lt;div&gt;項目。 內部&lt;div&gt;項目中，輸入：

@Gravatar.

@ 字元則是您已將 Razor 程式碼中使用的相同字元。 **Gravatar**是您正在使用的協助程式物件。

只要您輸入句號 （.） 時，WebMatrix 會顯示一份*方法*Gravatar 協助程式會提供 （函數）：

![Gravatar helper IntelliSense 下拉式清單](intro-to-web-pages-programming/_static/image10.png)

這項功能就所謂*IntelliSense*。 它會協助您的程式碼藉由提供適當的內容選項。 IntelliSense 會使用 HTML、 CSS、 ASP.NET 程式碼、 JavaScript 和 WebMatrix 支援其他語言。 它是另一項功能，可讓您更輕鬆地開發在 WebMatrix 中的網頁。

按 G 鍵盤，然後您會看到 IntelliSense 尋找 GetHtml 方法。 按 Tab 鍵。IntelliSense 會為您插入選取的方法 (GetHtml)。 輸入左括號，並請注意，自動加入右括弧。 在引號內的兩個括號之間輸入您的電子郵件地址。 如果您有 Gravatar 帳戶時，就會傳回您的個人資料圖片。 如果您沒有 Gravatar 帳戶，則會傳回預設映像。 當您完成時，該行看起來像這樣：

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

現在檢視的網頁瀏覽器中。 您的圖片或預設的影像會顯示，根據您所擁有的 Gravatar 帳戶。

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![預設映像](intro-to-web-pages-programming/_static/image12.png)

若要了解的功能為您執行協助程式，在瀏覽器中檢視頁面的來源。 以及 HTML 網頁中，您會看到影像項目，其中包含識別項。 這是協助程式轉譯成 [在您擁有的位置] 頁面的程式碼@Gravatar.GetHtml。 協助程式花費了您所提供，並產生與直接對 Gravatar，若要提供的帳戶取得回正確的映像的程式碼的資訊。

GetHtml 方法也可讓您自訂映像，藉由提供其他參數。 下列程式碼示範如何要求影像寬度和高度為 40 像素，並使用名為指定的預設映像**wavatar**如果指定的帳戶不存在。

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

此程式碼會產生類似下列的結果 （預設映像隨機會有所不同）。

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>接下來的下一步

為了讓本教學課程簡短，我們將焦點放在只有少數的基本概念。 當然，還有*許多*Razor 和 C# 更多。 您將了解更多您瀏覽這些教學課程。 如果您想要立即深入了解 Razor 和 C# 程式設計的層面，您可以閱讀更全面的簡介： [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkID=202890)。

下一個教學課程會介紹您使用的資料庫。 在該教學課程中，您會開始建立範例應用程式，可讓您列出您最愛的電影。

## <a name="complete-listing-for-testrazor-page"></a>TestRazor 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>TestRazorPart2 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>GravatarTest 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter 協助程式](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [上一頁](getting-started.md)
> [下一頁](displaying-data.md)
