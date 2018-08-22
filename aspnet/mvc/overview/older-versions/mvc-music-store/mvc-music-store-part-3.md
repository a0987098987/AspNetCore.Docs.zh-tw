---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 第 3 部分： 檢視和 ViewModels |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824895"
---
<a name="part-3-views-and-viewmodels"></a>第 3 部分： 檢視和 ViewModels
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。


到目前為止我們所只已傳回字串從控制器動作。 這是不錯的方式取得控制器的運作方式，了解但不是想要如何建置真實 web 應用程式。 我們想要更好的方法，以回到瀏覽我們的網站的瀏覽器產生 HTML，我們可以使用範本檔案更輕鬆地自訂 HTML 內容的其中一個寄回。 這正是檢視的。

## <a name="adding-a-view-template"></a>新增檢視範本

若要使用檢視範本，我們將變更 HomeController Index 方法要傳回 ActionResult，並且會傳回 View()，類似如下：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

上述的變更可讓您表示，而不是傳回字串，我們改為想要使用 「 檢視 」 來產生結果。

我們現在將我們的專案中加入適當的檢視範本。 若要這樣做，我們會將文字游標放在索引動作方法中，然後以滑鼠右鍵按一下並選取 [新增檢視]。 這會顯示 [新增檢視] 對話方塊：

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

[新增檢視] 對話方塊可讓我們快速且輕鬆地產生檢視範本檔案。 預設 [新增檢視] 對話方塊會預先填入的檢視範本來建立，使其符合使用它的動作方法的名稱。 因為我們使用我們 HomeController index （） 動作方法內的 [新增檢視] 內容功能表，上述的 [新增檢視] 對話方塊會有 「 索引 」 做為預先填入的預設檢視名稱。 我們不需要變更任何選項，此對話方塊上，因此按一下 [新增] 按鈕。

當我們按一下 [新增] 按鈕時，Visual Web Developer 會建立新的 Index.cshtml 為我們的檢視範本，在 \Views\Home 目錄中，建立資料夾，如果不存在。

![](mvc-music-store-part-3/_static/image2.png)

「 Index.cshtml 」 檔案的名稱和資料夾位置是很重要，並會遵循的預設 ASP.NET MVC 的命名慣例。 目錄名稱、 \Views\Home，比對名為 HomeController 的控制站-。 檢視範本名稱，而索引比對將會顯示檢視控制器動作方法。

ASP.NET MVC 可讓我們避免傳回檢視的情況下，我們在使用此命名慣例時，明確指定的名稱或檢視範本的位置。 它會預設轉譯 \Views\Home\Index.cshtml 檢視範本時我們我們 HomeController 中撰寫類似如下的程式碼：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer 中建立，並開啟 「 Index.cshtml 」 檢視範本之後我們按一下 [新增] 按鈕，在 [新增檢視] 對話方塊。 Index.cshtml 的內容如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

此檢視使用 Razor 語法，也就是比 ASP.NET Web Form 和舊版的 ASP.NET MVC 中使用 Web Form 檢視引擎更簡潔。 Web Form 檢視引擎在 ASP.NET MVC 3 中，仍然可用，但許多開發人員尋找 Razor 檢視引擎會非常適合 ASP.NET MVC 開發。

前三行設定使用 ViewBag.Title 頁面標題。 我們將查看其運作方式更詳細地推出，但首先讓我們更新文字標題文字，並檢視該頁面。 更新&lt;h2&gt;說: 「 這是首頁 」，如下所示的標記。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

執行應用程式會顯示我們新的文字顯示在首頁上。

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>對於常見的網站元素使用的版面配置

大部分的網站有許多的頁面之間共用的內容： 瀏覽、 頁尾、 標誌影像、 樣式表參考等。Razor 檢視引擎簡化這要使用一個稱為管理\_Layout.cshtml 自動/檢視/共用資料夾內建立的。

![](mvc-music-store-part-3/_static/image4.png)

按兩下這個資料夾，以檢視其內容，會如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

從我們的個別檢視的內容會顯示所@RenderBody（） 命令，以及任何通用的內容，我們想要出現在之外，可以新增至\_Layout.cshtml 標記。 我們希望我們首頁和存放區的區域，在網站中，所有的頁面上必須具有連結的通用標頭，因此我們將新增的正上方的範本我們 MVC Music 市集@RenderBody（） 陳述式。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>更新 樣式表

空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示驗證訊息的樣式。 部分其他 CSS 和影像來定義我們的網站，外觀與風格，因此我們將新增中現在提供我們的設計工具。

MvcMusicStore Assets.zip 可在內容目錄中包含更新過的 CSS 檔案和映像[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。 我們將在 Windows 檔案總管中選取這兩者，並卸除我們的解決方案內容的資料夾，在 Visual Web Developer 中，如下所示：

![](mvc-music-store-part-3/_static/image5.png)

系統會要求您確認是否您想要覆寫現有的 Site.css 檔案。 按一下 [是]。

![](mvc-music-store-part-3/_static/image6.png)

您的應用程式的內容資料夾現在會出現，如下所示：

![](mvc-music-store-part-3/_static/image7.png)

現在讓我們執行應用程式，並查看我們的變更在首頁上的外觀。

![](mvc-music-store-part-3/_static/image8.png)

- 讓我們檢閱變更的項目： HomeController 索引動作方法找到並顯示 [\Views\Home\Index.cshtmlView] 範本中，即使我們的程式碼會呼叫 「 傳回 View()"，因為我們的檢視範本會遵循標準的命名慣例。
- 首頁會顯示一個簡單的歡迎訊息 \Views\Home\Index.cshtml 檢視範本內定義的。
- 首頁會使用我們\_Layout.cshtml 範本，因此歡迎訊息內含 HTML 的標準網站的版面配置。

## <a name="using-a-model-to-pass-information-to-our-view"></a>使用模型來傳遞資訊給我們的檢視

只會顯示硬式編碼 HTML 的檢視範本並不會很有趣的網站。 若要建立動態網站，我們會改為想要將從我們的控制器動作的資訊傳遞至我們的檢視範本。

在模型-檢視-控制器模式中，模型是指一詞的物件代表應用程式中的資料。 通常，模型物件對應到您的資料庫中的資料表，但他們不需要。

控制器動作方法會傳回 ActionResult 可以傳遞至檢視的模型物件。 這可讓產生回應，以及檢視範本，然後將這項資訊傳遞所需的所有資訊完全封裝的控制站，要用來產生適當的 HTML 回應。 這是最容易了解藉由查看動作中，我們現在就開始。

首先我們要建立某些模型類別，來代表我們的存放區中的內容類型和專輯。 現在就開始建立內容類型的類別。 以滑鼠右鍵按一下您的專案內的 「 模型 」 資料夾中，選擇 「 加入類別 」 選項，並將檔案命名"Genre.cs 」。

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

然後加入所建立的類別公開的字串名稱屬性：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*注意： 萬一您好奇，{get; 設定;} 標記法正在使用 C# 的自動實作屬性功能。可以讓我們屬性的優點，而不需要我們宣告支援欄位。*

接下來，請遵循相同步驟來建立具有標題和內容類型屬性的專輯類別 （名為 Album.cs）：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

現在我們可以修改 StoreController，若要使用檢視會顯示動態資訊從我們的模型。 如果-供示範之用現在-我們名為我們的要求識別碼為基礎的相簿，我們可能會顯示下列檢視該資訊。

![](mvc-music-store-part-3/_static/image10.png)

我們一開始先變更存放區詳細資料動作，因此它會顯示為單一的專輯資訊。 將"using"陳述式加入至頂端**StoreControllers**類別，以包含 MvcMusicStore.Models 命名空間，因此我們不需要輸入 MvcMusicStore.Models.Album，每當我們想要使用的專輯類別。 該類別的 「 using 」 區段現在應該會出現，如下所示。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

接下來，我們將更新的詳細資料控制器動作，使它傳回 ActionResult，而不是字串，如同我們一樣 HomeController 的 Index 方法。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

現在我們可以修改傳回至檢視的專輯物件的邏輯。 本教學課程稍後我們將會從資料庫擷取資料 – 但現在我們將使用 「 虛擬資料 」 來開始。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*注意： 如果您不熟悉使用 C#，您可能會假設，使用 var 表示我們專輯的變數是晚期繫結。不正確 – C# 編譯器會使用根據什麼我們是指派給變數，以判斷該專輯的型別專輯和專輯型別為編譯本機專輯變數，因此我們編譯時期檢查和 Visual Studio 程式碼編輯器的型別推斷支援。*

我們現在來建立檢視範本來產生 HTML 回應中使用我們的專輯。 這樣做之前我們要建置專案，以便知道我們新建立的專輯類別的 [新增檢視] 對話方塊。 您可以建置專案選取 Debug⇨Build MvcMusicStore 功能表項目 （針對額外信用額度，您可以使用 Ctrl-Shift-B 捷徑來建置專案）。

![](mvc-music-store-part-3/_static/image11.png)

既然我們已經設定好我們支援的類別，我們可以開始建置我們的檢視範本。 以滑鼠右鍵按一下詳細資料的方法中，並從操作功能表選取 [新增檢視...]。

![](mvc-music-store-part-3/_static/image12.png)

我們要建立新的檢視範本像我們之前在 homecontroller。 因為我們會建立它從 StoreController 它將依預設會產生 \Views\Store\Index.cshtml 檔案中。

不同於之前，我們選取 [建立強型別] 檢視的核取方塊。 然後我們選取內 「 檢視資料類別 「 卸除 downlist 我們"Album"的類別。 這會導致 [新增檢視] 對話方塊，來建立檢視範本需要的物件會傳遞給它使用的 Album。

![](mvc-music-store-part-3/_static/image13.png)

當我們按一下 [新增] 按鈕時將會建立我們 \Views\Store\Details.cshtml 檢視範本包含下列程式碼。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

請注意第一行中，這表示此檢視為強型別到專輯類別。 Razor 檢視引擎了解，它已傳遞專輯物件，因此我們可以輕鬆地存取 模型屬性，即使已在 Visual Web Developer 編輯器中 IntelliSense 的優點。

更新&lt;h2&gt;標記讓它會專輯的 Title 屬性顯示的修改才會出現，如下所示的那一行。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

請注意，當您輸入超過此時限後的，會觸發 IntelliSense@Model關鍵字，顯示的屬性和專輯類別支援的方法。

讓我們現在重新執行我們的專案，並瀏覽市集/詳細資料/5 URL。 我們會看到類似如下的專輯的詳細資料。

![](mvc-music-store-part-3/_static/image14.png)

現在，我們將進行類似更新存放區瀏覽動作方法。 更新方法，使其傳回 ActionResult，並修改方法的邏輯，因此它會建立新的內容類型物件並將它傳回至檢視。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

以滑鼠右鍵按一下 瀏覽方法中，並從操作功能表中，選取 新增檢視 然後新增為強型別檢視的強型別加入內容類型的類別。

![](mvc-music-store-part-3/_static/image15.png)

更新&lt;h2&gt;檢視中的項目中的程式碼 （/Views/Store/Browse.cshtml) 顯示的內容類型資訊。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

現在讓我們重新執行我們的專案，並瀏覽至存放區/瀏覽？內容類型 = Disco 的 URL。 我們會看到顯示類似下方的瀏覽 頁面。

![](mvc-music-store-part-3/_static/image16.png)

最後，讓我們稍微複雜一點的更新，以**存放區索引**動作方法和檢視，以顯示在我們的存放區中的所有內容類型清單。 我們將使用清單的內容類型為我們的模型物件，而不是只是單一的內容類型來這麼做。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

以滑鼠右鍵按一下 存放區索引動作方法中，然後選取 新增檢視之前，請選取內容類型作為模型類別，以及按下 新增 按鈕。

![](mvc-music-store-part-3/_static/image17.png)

我們將會在第一次變更@model來指示檢視將會需要數個內容類型的宣告物件，而不是其中一個。 變更 /Store/Index.cshtml 讀取，如下所示的第一行：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

這會告知 Razor 檢視引擎，它會使用模型物件，可存放數個內容類型的物件。 我們正在使用 IEnumerable&lt;Genre&gt;而不是清單&lt;內容類型&gt;因為它是更泛型的讓我們可以將我們的模型類型稍後變更為任何支援 IEnumerable 介面的物件類型。

接下來，我們會執行迴圈內容類型中的物件模型中下列已完成的檢視程式碼所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

請注意，我們有完整的 IntelliSense 支援，我們輸入下列程式碼中，如此當我們輸入 「@Model。 」 我們看到所有方法和 IEnumerable 的型別內容類型所支援的屬性。

![](mvc-music-store-part-3/_static/image18.png)

在"foreach"迴圈，Visual Web Developer 知道的每個項目型別的內容類型，因此我們會看到 IntelliSense 為每個內容類型的類型。

![](mvc-music-store-part-3/_static/image19.png)

接下來，樣板功能會檢查內容類型的物件，並判斷，每個都會有名稱屬性，讓它執行迴圈，並將其輸出。它也會產生每個個別的項目編輯、 詳細資料和刪除連結。 我們會利用它，稍後我們存放區管理員 中，但現在我們想要改為有一個簡單的清單。

當我們執行應用程式，並瀏覽至 /Store 時，我們看到的計數 」 和 「 內容類型的清單會顯示。

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>新增頁面之間的連結

/Store URL，其中列出目前的內容類型只是以純文字形式列出的內容類型名稱。 讓我們變更這如此而不是純文字，我們改為需要內容類型名稱連結至適當的存放區/瀏覽 URL，以便按一下音樂內容類型，例如"Disco"會瀏覽至存放區/瀏覽？ 內容類型 = Disco URL。 我們無法更新我們 \Views\Store\Index.cshtml 檢視範本以使用這些連結的程式碼類似如下的輸出 **（不要鍵入中-我們將在其上改善）**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

這的確行得通，但可能會導致稍後疑難排解，因為它依賴硬式編碼字串。 比方說，如果我們想要重新命名該控制站時，我們需要透過我們的程式碼需要更新的連結尋找搜尋。

我們可以使用替代方法是利用 HTML 協助程式方法。 ASP.NET MVC 包含可從我們的檢視範本程式碼，以執行各種常見的工作，就像這樣的 HTML Helper 方法。 Html.ActionLink() helper 方法是一個特別有用，並可讓您更輕鬆建置 HTML &lt;&gt;連結，而惱人的細節，例如確定具有正確的 URL 路徑 URL 編碼。

Html.ActionLink() 有數個不同的多載，可讓您指定您的連結所需的資訊越。 在最簡單的情況下，您會提供連結文字和用戶端上按下超連結時，請移至動作方法。 比方說，我們可以連結至"/ Store /"連結文字 「 移至存放區索引 」 使用下列呼叫存放區詳細資料頁面上的 index （） 方法：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*注意： 在此情況下，我們不需要指定控制器名稱，因為我們只連結至目前的檢視形式呈現到相同控制器內的另一個動作。*

因此我們將使用另一個多載會採用三個參數的 Html.ActionLink 方法傳遞參數，不過，需要我們瀏覽 頁面的連結：

- 1. 連結文字，會顯示的內容類型名稱
- 2. 控制器動作名稱 （瀏覽）
- 3. 路由參數值，指定的名稱 （內容類型） 和值 （內容類型名稱）

將放在一起，以下我們要如何撰寫這些連結至存放區索引 檢視：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

現在當我們再次執行我們的專案，並存取 /Store/ URL，我們會看到內容類型的清單。 按下時，每個內容類型為超連結 – 它會把我們帶到我們/Store/瀏覽？ 內容類型 =*[內容類型]* URL。

![](mvc-music-store-part-3/_static/image3.jpg)

如需內容類型清單的 HTML 如下所示：

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-2.md)
> [下一頁](mvc-music-store-part-4.md)
