---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: 新增檢視 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: f55e558dd056e86bdd2310894959aef02a9d8de2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825337"
---
<a name="adding-a-view"></a>新增檢視
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


這一節我們要看看我們如何有 HelloWorldController 類別完全封裝給用戶端產生 HTML 回應中使用檢視範本檔案。

現在就開始使用檢視範本搭配我們 Index 方法。 我們的方法呼叫索引，且位於 HelloWorldController。 目前我們 index （） 方法傳回的訊息是控制器類別內的硬式編碼的字串。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

讓我們現在變更 Index 方法，而是看起來像這樣：

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

讓我們現在將檢視範本加入我們，我們可以使用我們的 index （） 方法的專案。 若要這樣做，請使用 Index 方法中間的某處滑鼠右鍵，按一下 新增檢視...

![影像](getting-started-with-mvc-part3/_static/image1.png)

這會顯示 [新增檢視] 對話方塊提供我們一些我們要如何建立可供我們 Index 方法檢視範本的選項。 現在，請勿變更任何項目，並只要按一下 [新增] 按鈕。

[![新增檢視對話方塊](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

按一下 [新增] 後，新的資料夾和新的檔案會出現在方案資料夾中，如下所示。 我現在會有 [HelloWorld] 資料夾之下的檢視和 Index.aspx 檔案在該資料夾內。

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

新的索引檔是也已經開啟，而且準備好進行編輯。 加入一些文字置於第一個&lt;h2&gt;Index&lt;/h2&gt;喜歡"Hello World"。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

執行應用程式，並瀏覽[ `http://localhost:xx/HelloWorld` ](http://localhostxx)一次在您的瀏覽器中。 在我們在此範例中的控制器的 Index 方法未執行任何工作，但未呼叫 「 傳回 View() 」 這表示我們想要使用檢視範本檔案來呈現的回應傳回給用戶端。 因為我們未明確指定要使用的檢視範本檔案的名稱，ASP.NET MVC 會預設使用 \Views\HelloWorld 資料夾內的 Index.aspx 檢視檔案。 我們現在看到我們硬式編碼在我們的檢視中的字串。

[![索引-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

看起來很好。 不過，請注意，瀏覽器的標題會顯示"Index"，並大的標題在頁面上顯示 「 我 MVC 應用程式。 」 讓我們將這些變更。

### <a name="changing-views-and-master-pages"></a>變更檢視表和主版頁面

首先，讓我們變更文字 「 我 MVC 應用程式。 」 該文字會共用，並出現在每一頁。 它實際上不會顯示只有一個位置，在我們的程式碼，即使是在我們的應用程式中的每個頁面上。 移至 /Views/Shared 資料夾，在 [方案總管] 中，開啟 Site.Master 檔。 這個檔案稱為主版頁面，而且共用 「 殼層 」 的所有其他網頁使用。

請注意 ContentPlaceholder"MainContent"一些文字，在此檔案。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

該預留位置是，您所建立的所有頁面會都顯示，主版頁面中的 「 包裝 」。 請嘗試變更標題，然後執行您的應用程式，並瀏覽多個頁面。 您會發現您的一項變更會出現多個頁面上。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

現在每個頁面上會有主要標題-這是 H1-「 我的 MVC 電影應用程式。 」 可處理的白色文字頂端那里跨所有頁面共用。

以下是 Site.Master 看來我們變更標題：

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

現在，讓我們變更索引頁面的標題。

開啟 /HelloWorld/Index.aspx。 沒有變更的兩個地方。 第一次，標題，會出現在瀏覽器中，則次要的標頭-也是 H2-標題。 我會使這些每個稍有不同，因此您可以看到的一些程式碼變更應用程式的哪個部分。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

執行您的應用程式，並瀏覽 /Movies。 請注意，瀏覽器標題、 主要標題和次要標題已變更。 它很容易就能只進行小變更的應用程式中的重大變更對您的檢視。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

我們的一些程式 （在此情況下 「 Hello World ！"的 「 資料 」 訊息） 很難透過自動程式化。 我們有 V （檢視），我們有 C （控制站），但沒有 M （模型） 尚未。 我們將稍後詳細說明如何建立資料庫，並從其擷取模型資料。

## <a name="passing-a-viewmodel"></a>傳遞 ViewModel

我們移至資料庫，並討論模型之前，不過，讓我們先討論 「 Viewmodel。 」 這些是代表檢視範本需要來呈現 HTML 回應傳回給用戶端的物件。 它們通常會建立控制器類別傳遞至檢視範本，並只可包含在檢視範本需要的資料與不再。

先前具有 HelloWorld 範例中，我們 Welcome() 動作方法所花費的名稱和 numTimes 參數，並將它輸出到瀏覽器。 與其讓控制器繼續直接呈現這個回應，讓我們改為進行小的類別來保存該資料，並接著將它傳遞至轉譯回 HTML 回應使用它的檢視範本。 如此一來，控制器會關心一件事並檢視範本另一個 – 讓我們可以維護清除 」 關注點分離 」 在我們的應用程式。

回到 HelloWorldController.cs 檔案並加入新的 「 WelcomeViewModel 」 類別，變更您的控制器內的 歡迎使用方法。 以下是完整的 HelloWorldController.cs 與新的類別，在相同的檔案。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

即使是在多行上，我們歡迎畫面的方法其實只有兩個程式碼陳述式。 第一個陳述式的 ViewModel 物件，以及第二個傳遞到我們兩個參數封裝在檢視上產生的物件。

現在我們需要 褖畫惎檢視範本 ！ 以滑鼠右鍵按一下該 褖畫惎方法中，然後選取 加入檢視。 此時，我們會核取 [建立強型別檢視]，並從下拉式清單中選取我們 WelcomeViewModel 類別。 這個新的檢視只會知道 WelcomeViewModels 和任何其他類型的物件。

> *注意： 您必須編譯一次之後才會出現在下拉式清單中加入您的 WelcomeViewModel。*


以下是您的 [新增檢視] 對話方塊的外觀。 按一下 [新增] 按鈕。 ![新增檢視圈選起來](getting-started-with-mvc-part3/_static/image10.png)

新增下列程式碼底下&lt;h2&gt;中新的 Welcome.aspx。 我們將進行迴圈，並認識使用者說我們應該無限次 ！

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

另外請注意，因為您正在輸入，因為我們說過這 WelcomeViewModel 相關檢視 （已婚，記得嗎？），我們得到很有幫助的 Intellisense，每次我們參考我們的模型物件，在以下螢幕擷取畫面所示：

[![NumTime 原始程式碼](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

執行應用程式，並瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`一次。 現在我們從 URL 取得資料，則會自動傳遞到控制器，控制器會封裝將資料分成 ViewModel，並將拖曳至我們的檢視該物件傳遞。 檢視比對使用者顯示為 HTML 的資料。

[![歡迎使用-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

這是一種模型，"M"，但不是資料庫類型。 讓我們看什麼我們已了解並建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part2.md)
> [下一頁](getting-started-with-mvc-part4.md)
