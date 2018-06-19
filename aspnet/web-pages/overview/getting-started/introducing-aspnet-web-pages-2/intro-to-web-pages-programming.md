---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 導入 ASP.NET Web Pages-程式設計基本概念 |Microsoft 文件
author: tfitzmac
description: 本教學課程可讓您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。 您將學習： 基本 'Razor' 語法用於 pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "33839282"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>導入的 ASP.NET Web Pages-程式設計基本概念
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程可讓您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。
> 
> 您將學習：
> 
> - 您用於 ASP.NET Web Pages 中的程式設計基本"Razor"語法。
> - 某些基本 C# 中，也就是您將使用的程式設計語言。
> - 網頁的一些基本程式設計概念。
> - 如何安裝封裝 （包含預先建置的程式碼的元件） 以使用與您的網站。
> - 如何使用*helper*執行常見的程式設計工作。
>   
> 
> 功能/技術討論：
> 
> - NuGet 和封裝管理員。
> - `Gravatar`協助程式。


本教學課程是主要是一項工作所介紹的程式設計的語法，您將使用的 ASP.NET Web Pages。 您將了解*Razor 語法*和在 C# 撰寫的程式碼的程式設計語言。 您在上一個教學課程中，有了解這種語法在本教學課程中，我們將說明詳細的語法。

我們保證本教學課程包含程式設計，您會在單一的教學課程中，看到，而且它是唯一的教學課程的大部分*只*需程式設計。 在此集合中其餘的教學課程，您實際上會建立執行有趣的事情的頁面。

您也將了解*helper*。 協助程式是一種元件 — 封裝最多段程式碼 — 您可以加入至頁面。 協助專家會執行，否則可能會很繁瑣或是以手動方式執行複雜的工作。

## <a name="creating-a-page-to-play-with-razor"></a>建立要玩 Razor 的網頁

本節中您將播放位元 razor 因此您可以取得有意義的基本語法。

如果尚未執行，請啟動 WebMatrix。 您將使用您在上一個教學課程中建立的網站 ([快速入門開始使用網頁](https://go.microsoft.com/fwlink/?LinkId=251578))。 若要重新開啟它，請按一下**我網站**選擇**WebPageMovies**:

![WebMatrix 啟動畫面，顯示反白顯示 我的站台與開啟的站台選項](intro-to-web-pages-programming/_static/image1.png)

選取**檔案**工作區。

在 功能區中，按一下 **新增**建立頁面。 選取**CSHTML**並命名新的頁面*TestRazor.cshtml*。

按一下 [確定 **Deploying Office Solutions**]。

將下列複製到檔案，完全取代已經存在。

> [!NOTE]
> 當您到頁面上，複製程式碼或標記與範例時，縮排和對齊方式可能無法與本教學課程中的相同。 如何在程式碼執行，不過並不影響縮排和對齊方式。


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>檢查頁面範例

您所看到的大部分是一般的 HTML。 不過，在頂端沒有這個程式碼區塊：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

請注意下列事項有關這個程式碼區塊：

- @ 字元會告知 ASP.NET，以下是 Razor 程式碼，而不是 HTML。 ASP.NET 會將所有項目之後 @ 字元，直到它再次會碰到一些 HTML 程式碼。 (在此情況下的&lt;！DOCTYPE&gt;項目。
- 大括號 （{和}） 括住的 Razor 程式碼區塊，如果程式碼有一行以上。 大括號會告知 ASP.NET，該區塊的程式碼開始和結束的位置。
- / / 字元標記註解，也就是說，不會執行的程式碼的一部分。
- 每個陳述式必須以分號 （;） 結束。 （沒有註解，雖然。）
- 您可以儲存在值*變數*，您所建立 (*宣告*) 與關鍵字時間差異 當您建立變數時，您為它命名，可包含字母、 數字和底線 (\_)。 變數名稱不能以數字開頭，且無法使用 （例如 var) 程式設計關鍵字的名稱。
- 以引號括住字元字串 （例如"ASP.NET 」 和 「 網頁 」）。 （它們必須是雙引號）。數字不是以引號。
- 空白字元引號之外並不重要。 分行符號大多不重要;例外情況是您無法跨行分割以引號的字串。 縮排和對齊方式不重要。

較不明顯的這個範例是所有程式碼是區分大小寫。 這表示變數 TheSum 是不同的變數數目多於 theSum 可能名為的變數數目。 同樣地，var 關鍵字，但不是 Var。

### <a name="objects-and-properties-and-methods"></a>物件和屬性和方法

就 DateTime.Now 的運算式。 簡單地說，日期時間是*物件*。 物件是您可以使用程式的功能 — 頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄、 等。物件有一或多個*屬性*描述它們的特性。 文字方塊物件都有文字屬性 （和其他項目）、 要求物件具有 Url 屬性 （以及其他）、 電子郵件訊息具有 From 屬性和 To 屬性，依此類推。 物件也有*方法*是他們可以執行的 「 動詞"。 您會經常使用物件許多。

您可以看到的範例中，日期時間是物件，可讓您程式的日期和時間。 它有一個名為現在會傳回目前日期和時間屬性。

### <a name="using-code-to-render-markup-in-the-page"></a>使用程式碼轉譯頁面中的標記

在網頁主體中，請注意下列各項：

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

同樣地，@ 字元會告知 ASP.NET，以下是程式碼，而不是 HTML。 在標記中您可以新增後面的程式碼運算式，而且 ASP.NET 會在該點轉譯該運算式右邊的值。 在範例中，@a會呈現適當的值是名為的變數，@product呈現無論是在變數為指定的產品，並依此類推。

您可以使用不到變數，不過。 在少數情況下，@ 字元前面的運算式：

- @(\*b） 呈現任何為變數中的產品 a 和 b。 (\*運算子表示乘法。)
- @(技術 +""+ 產品) 呈現它們串連和之間加入空格之後的變數技術與產品中的值。 運算子相同的加上數字的字串將字串串連運算子 （+）。 您是否正在與數字或字串，並會使用正確的動作，通常可以判斷 ASP.NET + 運算子。
- @Request.Url 會轉譯要求物件的 Url 屬性。 要求物件包含從瀏覽器、 目前要求的相關資訊，當然 Url 屬性包含目前要求的 URL。

此範例也被為了顯示您可以執行不同的方式運作。 您可以進行上方的程式碼區塊中的計算，請將結果放入變數，並接著呈現標記中的變數。 或者，您可以執行在標記中的運算式右邊的計算。 您使用的方法取決於您正在進行的作業，以及在某種程度上您自己的喜好設定。

### <a name="seeing-the-code-in-action"></a>查看執行中的程式碼

以滑鼠右鍵按一下檔案的名稱，然後選擇 **瀏覽器中啟動**。 您會看到所有的值和運算式在網頁中解決瀏覽器的網頁。

!['TestRazor' 網頁瀏覽器中執行](intro-to-web-pages-programming/_static/image2.png)

請查看瀏覽器中的來源。

![在瀏覽器中測試 Razor 網頁原始檔](intro-to-web-pages-programming/_static/image3.png)

如您預期從您在上一個教學課程中的體驗，Razor 程式碼都是在的頁面。 您會看到所有會實際顯示值。 當您執行頁面時，您實際上正在要求 WebMatrix 內建的 web 伺服器。 收到要求時，ASP.NET 會解析所有的值與運算式，並將其值轉譯成網頁。 它接著會傳送至瀏覽器的網頁。

> [!TIP] 
> 
> **Razor 和 C#**
> 
> 到目前為止我們所說的內容，您正在使用 Razor 語法。 為 true，但它不會完成的劇本。 實際的程式設計語言，您使用稱為*C#*。 C# 建立由 Microsoft 前以來，已成為主要的程式設計語言來建立 Windows 應用程式。 您已經看到如何命名的變數，以及如何建立陳述式等相關的所有規則都是實際的 C# 語言的所有規則。
> 
> Razor 更明確地說是指少量慣例，您將此程式碼內嵌到頁面。 例如，使用標記程式碼以在網頁中的以及使用的慣例 @ {} 將內嵌程式碼區塊是網頁 Razor 層面。 Helper 也會被視為 Razor 的一部分。 多個地方比只 ASP.NET Web Pages 中使用 razor 語法。 （例如，它使用 ASP.NET MVC 檢視表中）。
> 
> 我們曾經提到這因為如果您尋找資訊相關的程式設計 ASP.NET Web Pages，您會看到許多 Razor 的參考。 不過，這些參考大量不適用於所要這樣做，因此可能會造成混淆。 而且事實上，許多程式設計問題的真正將會使用 C# 或使用 ASP.NET 的相關。 因此如果您特別尋找 Razor 的相關資訊，您可能會找不到您所需要的答案。


## <a name="adding-some-conditional-logic"></a>加入一些條件式邏輯

其中一個相關網頁中使用程式碼很棒的功能。 您可以變更會發生什麼事根據各種條件 在本教學課程的這個部分，您將試問一些方法，若要變更頁面中顯示的內容。

簡單與有點不自然，以便我們可以專注於條件式邏輯，會將此範例。 您將建立的網頁會執行此作業：

- 取決於它是否在第一次顯示網頁時，或您已按一下按鈕以送出此頁面，請在頁面上顯示不同的文字。 這會是第一個條件的測試。
- 為某個值傳入查詢字串的 URL (http://...?show=true) 時，才會顯示訊息。 這會是第二個條件的測試。

在 WebMatrix 中，建立網頁並將其命名*TestRazorPart2.cshtml*。 (在功能區中，按一下**新增**，選擇**CSHTML**，檔案名稱，然後按一下**確定**。)

以下列內容取代該頁面的內容：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

在最上方的程式碼區塊會初始化名為包含某些文字訊息的變數。 在網頁主體中，訊息變數的內容會顯示在&lt;p&gt;項目。 標記也包含&lt;輸入&gt;項目來建立**送出** 按鈕。

執行頁面，以查看它如何運作。 現在，基本上它是靜態的頁面上，即使您按一下**送出** 按鈕。

返回至 WebMatrix。 在程式碼區塊中，加入下列反白顯示的程式碼*之後*初始化訊息的一行：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If {} 區塊

您剛才加入的 if 條件。 在程式碼中必須有條件的結構如下：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

要測試的條件是在括號中。 它必須是值或傳回 true 或 false 的運算式。 如果條件為 true，ASP.NET 會執行陳述式或大括號內的陳述式。 (這些是*然後*屬於*if-then*邏輯。)如果條件為 false，則會略過的程式碼區塊。

以下是一些您可以在測試的條件的範例陳述式：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

您可以使用測試變數針對值或運算式<em>邏輯運算子</em>或<em>比較運算子</em>： 等於 （= =）、 大於 (&gt;)、 小於 (&lt;)，大於或等於 (&gt;=)，且小於或等於 (&lt;=)。 ！ = 運算子表示不等於 — 例如，如果 (！ = 0) 表示<em>如果</em> <em>不等於 0</em>。

> [!NOTE]
> 請確定您會注意到，等於 （= =） 比較運算子不相同 =。 = 運算子只能用來將值指派 (var = 2)。 如果您混合使用這些運算子，您將可取得錯誤或就會顯示一些奇怪的結果。


若要測試是否有為 true，則完整的語法就是 if(IsDone == true)。 但是，您也可以使用捷徑 if(IsDone)。 如果沒有任何比較運算子，ASP.NET 會假設您要測試為 true。

！ 單獨使用的運算子表示邏輯 NOT。 例如，條件 if （！IsPost) 表示*如果 IsPost 不是 true*。

您可以使用邏輯 AND 結合條件 (&amp; &amp;運算子) 或邏輯 OR (| | 運算子)。 例如，if 的最後一個條件前面的範例表示*如果 FileProcessingIsDone 未設為 true AND displayMessage 設為 false*。

### <a name="the-else-block"></a>在 else 區塊

最後一有關 if 區塊： 如果區塊後面可以接著其他區塊。 其他區塊可以是您必須在條件為 false 時，執行不同的程式碼。 以下是簡單的範例：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

在其中使用其他區塊是有用的這一系列之後的教學課程中，您會看到一些範例。

### <a name="testing-whether-the-request-is-a-submit-post"></a>測試是否要求已提交 (post)

還有更多，但是讓我們回到範例中，有條件 if(IsPost) {...}。 IsPost 是實際的目前頁面的屬性。 第一次要求頁面時，IsPost 傳回 false。 不過，如果您按一下按鈕或否則送出此頁面，也就是將它張貼 — IsPost 傳回 true。 因此 IsPost 可讓您決定是否要處理的送出表單。 （根據 HTTP 指令動詞，如果要求是 「 取得 」 作業，IsPost 傳回 false。 如果要求是 POST 作業，IsPost 傳回 true。）在之後的教學課程中您將使用輸入表單，其中這項測試特別實用。

執行網頁。 因為這是第一次收到要求 頁面上，您會看到"是第一次要求頁面 」。 初始化訊息的變數的值為該字串。 If(IsPost) 測試，但會傳回 false 時，使 if 內的程式碼封鎖無法執行。

按一下**送出** 按鈕。 要求頁面時一次。 為之前，訊息變數設定為"This is 第一次..."。 但這次測試 if(IsPost) 會傳回 true，因此 if 內的程式碼會封鎖執行。 程式碼變更為不同的值，這是要呈現在標記中的訊息變數的值。

現在，加入 if 標記中的條件。 下面&lt;p&gt;包含項目**送出**按鈕，加入下列標記：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

您要加入標記內的程式碼，所以您必須開始使用@。 If 就類似您先前加入程式碼區塊中的測試。 在大括弧，不過，您要加入一般 HTML — 至少，它是一般直到到達@DateTime.Now。 這是另一個一點點的 Razor 程式碼，因此一次您必須在它前面加上 @。

此處的重點是，如果您可以加入條件中的程式碼區塊頂端，並在標記中。 如果您使用 [if] 頁面上，將區塊內部的程式行的主體中的條件可以是標記或程式碼。 在此情況下，每當您混用標記和程式碼，則為 true，因為您必須使用，讓它清除 ASP.NET 程式碼的位置。

執行網頁，然後按一下**送出**。 這次您不只看到不同的訊息時您提交 （「 現在您已送出..."），但您會看到新的訊息，列出的日期和時間。

![Test Razor 2' 的網頁瀏覽器中執行後顯示的時間戳記送出](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>測試值的查詢字串

一個詳細的測試。 此時，您會將 if 測試值的區塊，名為 「 可能會傳遞查詢字串中的顯示。 (這種方式： `http://localhost:43097/TestRazorPart2.cshtml?show=true`)，以便顯示您已顯示，您要變更頁面 （"This is 第一次..."等） 如果顯示的值為 true，才會顯示。

在底端 （但內部） 程式碼區塊頂端的頁面上，加入下列內容：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

完整的程式碼區塊現在看起來與下列範例。 （請記住，當您將程式碼複製到您的頁面時，縮排可能會有不同。 但是，不會影響程式碼的執行。）

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

新的程式碼區塊中初始化變數，名為區隔名稱為 false。 然後再執行 if 測試來尋找查詢字串中的值。 當您第一次要求頁面時，它會有與下列類似的 URL:

`http://localhost:43097/TestRazorPart2.cshtml`

程式碼會判斷 URL 是否包含名為查詢字串，例如 URL 的這個版本中顯示的變數：

`http://localhost:43097/TestRazorPart2.cshtml`？ 顯示 = true

測試本身會查看要求物件的查詢字串屬性。 如果查詢字串包含的項目的顯示，而且該項目的設為 true，如果執行區塊，並區隔名稱變數設定為 true。

沒有一墩，因為您可以看到。 如其名，查詢字串是一個字串。 不過，您可以只測試為 true 和 false 如果您要測試的值是布林 (true/false) 值。 您可以測試查詢字串中的顯示變數的值之前，您必須將它轉換成布林值。 是 AsBool 方法所執行的工作，它接受字串做為輸入，並將它轉換成布林值。 很明顯地，如果字串為"true"，AsBool 方法會將該值轉換為 true。 如果字串的值可以是任何其他項目，AsBool 傳回 false。

> [!TIP] 
> 
> **資料型別和 as （） 方法**
> 
> 只說過為止，當您建立變數時，您使用關鍵字的時間差異 這不是整個本文中，不過。 才能操作值 — 若要加入數字，或串連字串，或比較日期，或測試的 true/false-C# 使用已適當值的內部表示法。 C# 可以*通常*找出該表示應該為何 (也就是什麼*類型*資料) 根據您所做的值。 不過，它不能進行該。 如果沒有，您有幫助藉由明確指示如何 C# 應該代表的資料。 AsBool 方法會執行的它會告訴 C#"true"或"false"的字串值應視為布林值。 有類似的方法，可做為其他型別，代表字串，類似 AsInt （視為整數）、 AsDateTime （視為日期/時間）、 AsFloat （視為浮點數），等等。 當您使用 （） 方法，如果要求 C# 無法表示的字串值時，您會看到錯誤。


在網頁標記中，移除或註解這個項目 （此處會顯示註解化）：

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

權限，讓您移除或註解的文字，加入下列內容：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

如果測試顯示，區隔名稱變數為 true，是否轉譯&lt;p&gt;訊息變數的值的項目。

### <a name="summary-of-your-conditional-logic"></a>您的條件式邏輯的摘要

如果您不確定完全的功能在您剛完成動作，此為摘要。

- 訊息變數會初始化為預設字串 ("This is 第一次...")。
- 如果頁面要求 (post) 提交的結果，訊息的值會變成 「 現在您上傳...」
- 區隔名稱變數會初始化為 false。
- 如果查詢字串包含？ 顯示 = 分開變數會設定為 true，則為 true。
- 在標記中，如果為 true，區隔名稱&lt;p&gt;項目會呈現，其中顯示訊息的值。 （如果分開為 false，不會呈現在該點的標記中。）
- 在標記中，如果要求是使用 post 要求&lt;p&gt;項目會呈現可顯示的日期和時間。

執行網頁。 沒有任何訊息，因為分開為 false，所以在標記中 if(showMessage) 測試會傳回 false。

按一下 [提交]。 您會看到日期和時間，但仍有任何訊息。

在瀏覽器中，移至 [URL] 方塊並加上下列 URL 的結尾:？ 顯示 = true，然後按 Enter。

![顯示查詢字串的瀏覽器中測試 Razor 2 頁面](intro-to-web-pages-programming/_static/image5.png)

頁面會再次顯示。 （因為您變更 URL，這是新的要求時，未送出）。按一下**送出**一次。 訊息會顯示一次，為日期和時間。

![查詢字串時，送出後 test Razor 2' 的頁面](intro-to-web-pages-programming/_static/image6.png)

在 URL 中，變更嗎？ 顯示 = true？ 顯示 = false，然後按 Enter 鍵。 重新送出此頁面。 頁面是回到您的啟動方式 — 沒有任何訊息。

如前文所述，此範例中的邏輯是有點不自然。 不過，如果要提供的許多頁面，並會需要一或多個您所見的表單這裡。

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>安裝 （顯示 Gravatar 映像） 協助程式

人們通常想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。 範例： 顯示的圖表資料。將 Facebook [讚] 按鈕放在頁面上。傳送電子郵件是來自您的網站。裁剪或調整大小的影像。PayPal 用於您的網站。 若要可讓您輕鬆執行這類項目，ASP.NET Web 網頁可讓您使用*helper*。 協助程式是您安裝站台和，可讓您執行一般工作所使用的 Razor 程式碼只需要幾行的元件。

ASP.NET Web 網頁會有幾個內建的協助程式。 然而，許多協助程式中的封裝 （增益集） 提供透過 NuGet 套件管理員。 NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。

在本教學課程的這個部分，您將安裝可讓您顯示 Gravatar （「 全域可辨識的顯示圖片 」） 映像的 helper。 您將學習兩件事。 其中一個是如何尋找及安裝協助程式。 您也將學習如何協助專家容易做到否則您需要使用許多您必須自行撰寫程式碼來執行。

您可以註冊自己 Gravatar Gravatar 網站在[ http://www.gravatar.com/ ](http://www.gravatar.com/)，但不需要建立 Gravatar 帳戶來執行本教學課程的這個部分。

在 WebMatrix 中，按一下 [ **NuGet** ] 按鈕。

![在 WebMatrix NuGet Gallery 對話方塊](intro-to-web-pages-programming/_static/image7.png)

這會啟動 NuGet 封裝管理員，並顯示可用的封裝。 (並非所有封裝都協助程式; 部分將功能加入至本身的 WebMatrix，部分其他範本，且等。)您可能會收到有關版本不相容的錯誤訊息。 您可以忽略此錯誤訊息，依序按一下**確定**並繼續進行本教學課程。

![在 WebMatrix NuGet Gallery 對話方塊](intro-to-web-pages-programming/_static/image8.png)

在 [搜尋] 方塊中，輸入 「 asp.net 協助程式 」。 NuGet 會顯示符合搜尋條件的封裝。

![在 WebMatrix 中顯示封裝的 NuGet Gallery](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Helpers Library 包含可簡化許多常見的工作，包括使用 Gravatar 映像的程式碼。 選取**ASP.NET Web Helpers Library**封裝，然後按一下 **安裝**啟動安裝程式。 選取**是**當系統詢問您要安裝封裝，並接受條款才能完成安裝。

就這麼容易！ NuGet 下載並安裝任何項目，包括任何可能需要的其他元件 (*相依性*)。

如果基於某些原因，您必須解除安裝 helper，處理程序是非常類似。 按一下**NuGet**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。

## <a name="using-a-helper-in-a-page"></a>在網頁中使用的 Helper

現在您可以使用您剛才安裝的協助程式。 加入網頁中的協助程式的程序是類似的大多數協助程式。

在 WebMatrix 中，建立網頁並將其命名*GravatarTest.cshml*。 （您要建立特殊的頁面，來測試協助程式，但您可以使用協助程式在您的網站中的任何網頁中）。

內部&lt;主體&gt;項目，加入&lt;div&gt;項目。 內部&lt;div&gt;元素中，輸入下列命令：

@Gravatar.

@ 字元是您以標示 Razor 程式碼所使用的相同字元。 **Gravatar**是您正在使用的協助程式物件。

只要您輸入句號 （.） 時，WebMatrix 會顯示一份*方法*Gravatar helper 提供 （函數）：

![Gravatar helper IntelliSense 下拉式清單](intro-to-web-pages-programming/_static/image10.png)

這項功能稱為*IntelliSense*。 它會藉由提供適當的內容選項協助您的程式碼。 IntelliSense 會搭配 HTML、 CSS、 ASP.NET 程式碼、 JavaScript 和 WebMatrix 中支援其他語言。 它是另一項功能，可讓您更輕鬆地開發在 WebMatrix 的網頁。

按 G 上的鍵盤，而且您會看到 IntelliSense 尋找 GetHtml 方法。 按 Tab 鍵。IntelliSense 會為您插入選取的方法 (GetHtml)。 輸入左括號，並注意右括號會自動加入。 在引號內的兩個括號之間輸入電子郵件地址。 如果您有 Gravatar 帳戶時，將會傳回設定檔圖片。 如果您沒有 Gravatar 帳戶，則會傳回預設映像。 當您完成時，行看起來像這樣：

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

立即在瀏覽器中檢視網頁。 您的圖片或預設影像會顯示，視您是否擁有 Gravatar 帳戶而定。

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![預設映像](intro-to-web-pages-programming/_static/image12.png)

若要了解的專家進行為您的動作，在瀏覽器中檢視網頁的來源。 您必須在頁面的 HTML，以及您會看到影像項目，其中包含識別項。 這是協助專家轉譯成 [在您需要的位置] 頁面的程式碼@Gravatar.GetHtml。 協助程式花費了提供，而且產生的程式碼，交談直接 Gravatar 以提供的帳戶取得回正確的映像的資訊。

GetHtml 方法也可讓您藉由提供其他參數自訂映像。 下列程式碼示範如何要求影像寬度和高度為 40 像素，並使用名為指定的預設映像**wavatar**如果指定的帳戶不存在。

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

此程式碼會產生類似下列的結果 （預設映像隨機不定）。

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>接下來

為了讓此教學課程簡短，我們將焦點放在只有少數的基本概念。 當然，沒有*許多*詳細 Razor 和 C#。 您將學習更多您瀏覽這些教學課程。 如果您想要以深入了解 Razor 和 C# 的程式設計方面現在，您可以閱讀，更完整的介紹： [ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkID=202890)。

下一個教學課程介紹您使用的資料庫。 在該教學課程中，您會開始建立範例應用程式，可讓您列出您最愛的電影。

## <a name="complete-listing-for-testrazor-page"></a>TestRazor 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>TestRazorPart2 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>GravatarTest 頁面的完整清單

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter helper](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [上一頁](getting-started.md)
> [下一頁](displaying-data.md)
