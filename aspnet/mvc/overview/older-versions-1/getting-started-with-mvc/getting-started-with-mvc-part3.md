---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "加入檢視 |Microsoft 文件"
author: shanselman
description: "這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 509dd301eef7c00431eae194a0df69d70e6d80f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>加入的檢視
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們即將看看如何我們有 HelloWorldController 類別完全封裝產生的 HTML 回應傳回給用戶端使用檢視範本檔案。

讓我們開始使用我們的方法中的索引檢視範本。 我們的方法呼叫的索引，且 HelloWorldController。 目前我們 index （） 方法傳回的訊息是在控制器類別內的硬式編碼的字串。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

讓我們現在變更索引方法，而是看起來像這樣：

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

我們現在將加入檢視範本至受測專案，我們可以使用我們的 index （） 方法。 若要這樣做，以滑鼠右鍵按一下使用滑鼠中間索引方法的某個位置並按一下 新增檢視...

![影像](getting-started-with-mvc-part3/_static/image1.png)

這會顯示我們提供一些選項，我們要如何建立可供我們的方法中的索引檢視表範本的 [新增檢視] 對話方塊。 現在請不要變更任何項目，並只要按一下 [新增] 按鈕。

[![新增檢視對話方塊](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

按一下 [新增] 後，新的資料夾和新的檔案會出現在方案資料夾中，如下所示。 我現在會有 [HelloWorld] 資料夾之下的檢視表和 Index.aspx 檔案資料夾中。

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

新的索引檔也是已開啟並且可供編輯。 加入一些文字，第一個下的&lt;h2&gt;索引&lt;/h2&gt;喜歡"Hello World"。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

執行您的應用程式，請瀏覽[ `http://localhost:xx/HelloWorld` ](http://localhostxx)瀏覽器中一次。 索引中的方法在此範例中我們控制站未做任何的工作，但未呼叫"傳回 View()"這表示我們想要使用的檢視範本檔案來呈現回應傳回給用戶端。 因為我們並未明確指定要使用的檢視範本檔案的名稱，ASP.NET MVC 預設 \Views\HelloWorld 資料夾內使用 Index.aspx 檢視檔案。 現在我們可以看到我們硬式編碼在我們的檢視中的字串。

[![索引-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

看起來很好。 不過，請注意，"Index"的瀏覽器的標題指出大的標題，在頁面上顯示 「 我 MVC 應用程式。 」 讓我們將這些變更。

### <a name="changing-views-and-master-pages"></a>變更檢視和主版頁面

首先，我們來變更文字 「 我 MVC 應用程式。 」 該文字共用，並顯示每一頁上。 它實際上不會顯示只有一個位置中的程式碼，即使它是我們的應用程式中的每一頁上。 移至 [方案總管] 中的 /Views/Shared 資料夾，然後開啟 Site.Master 檔。 這個檔案稱為主版頁面，它是共用 「 殼層 」，其他所有網頁都使用。

這個檔案會注意到 ContentPlaceholder"MainContent"這些文字。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

該預留位置是在您建立的所有頁面會都顯示，主版頁面中的 「 包裝 」。 請嘗試變更標題，然後執行您的應用程式，並瀏覽多個頁面。 您會發現您的一項變更會出現多個頁面上。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

現在每一頁會有主要的標題-這是 H1-的 「 我 MVC 電影應用程式。 」 可處理的白色文字上方那里共用的所有頁面。

以下是我們變更標題整個 Site.Master:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

現在，讓我們來變更索引頁的標題。

開啟 /HelloWorld/Index.aspx。 沒有變更的兩個地方。 首先，標題，會出現在瀏覽器，然後次要標頭-也是 H2-的標題。 我要進行每個稍有不同，因此您可以看到哪些程式碼變更的應用程式的一部分。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

執行您的應用程式，並瀏覽 /Movies。 請注意，瀏覽器標題、 標題主要和次要的標題已變更。 很容易就能讓您檢視中進行小變更您的應用程式項重大變更。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

我們一點點的 「 資料 」 （在此情況下的"Hello World ！" 訊息） 很難透過自動程式碼。 我們有 V (Views)，我們有 C （控制站），但沒有 M （模型） 尚未。 我們將稍後逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-a-viewmodel"></a>傳遞 ViewModel

我們請移至資料庫，並討論模型之前，不過，讓我們先討論 「 ViewModels。 」 這些是代表檢視範本來呈現 HTML 回應傳回給用戶端需要的物件。 它們通常建立由控制器類別傳遞給檢視範本，並應該只包含需要檢視表範本的資料的動作。

先前使用 HelloWorld 範例中，我們 Welcome() 動作方法的名稱和 numTimes 參數，並輸出至瀏覽器。 而非保有繼續直接轉譯這個回應的控制站，讓我們反而是讓保存該資料，然後將其傳遞至回轉譯 HTML 回應使用它的檢視範本的小型類別。 如此一來，控制器關於一件事並檢視範本另 – 讓我們能夠維護清除 」 的重要性分離 」 應用程式中。

返回 HelloWorldController.cs 檔案並加入新的 「 WelcomeViewModel 」 類別並變更您的控制器內的 歡迎使用方法。 以下是完整的 HelloWorldController.cs 與新的類別，在相同的檔案。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

即使是在多行上，我們褖畫惎方法實際上是只有兩個程式碼陳述式。 我們到 ViewModel 物件，而第二個階段的兩個參數封裝的第一個陳述式在檢視上產生的物件。

現在我們需要褖畫惎檢視範本 ！ 以滑鼠右鍵按一下該褖畫惎方法中，然後選取 加入檢視。 這次我們會檢查 「 建立強型別檢視 」，並且從下拉式清單中選取 WelcomeViewModel 類別。 這個新的檢視只會知道 WelcomeViewModels 和其他類型的物件。

> *注意： 您將需要編譯一次後才會出現在下拉式清單中加入您的 WelcomeViewModel。*


以下是您加入檢視的對話方塊外觀應該為何。 按一下 [新增] 按鈕。 ![加入檢視圈選起來](getting-started-with-mvc-part3/_static/image10.png)

此程式碼下新增&lt;h2&gt;您新 Welcome.aspx 中。 我們將進行迴圈，並依需求多次使用者可指定我們應該看看 ！

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

另外而且請注意，因為您正在輸入，因為我們會告知這 WelcomeViewModel 有關檢視 （未婚，記得） 嗎？ 我們取得很有幫助我們每次參考我們所見以下螢幕擷取畫面中的模型物件的 Intellisense:

[![NumTime 原始程式碼](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

執行您的應用程式，請瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`一次。 現在我們將帶資料從 URL，它會自動傳遞到我們的控制器，Controller ViewModel 資料的封裝，並傳遞到我們檢視該物件。 檢視比對使用者顯示為 HTML 的資料。

[![歡迎使用-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

這是一種模式下，"M"，但資料庫類型。 讓我們來看我們所學的內容和建立資料庫的電影。

>[!div class="step-by-step"]
[上一頁](getting-started-with-mvc-part2.md)
[下一頁](getting-started-with-mvc-part4.md)
