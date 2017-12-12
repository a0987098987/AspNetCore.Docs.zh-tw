---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "第 3 部分： 檢視和 ViewModels |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a>第 3 部分： 檢視和 ViewModels
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。


到目前為止我們所只已傳回字串從控制器動作。 這是不錯的方法，以了解如何使用的控制站，但不是想要如何建置實際的 web 應用程式。 我們想要更好的方法來回到瀏覽我們的網站的瀏覽器產生 HTML – 其中一個，我們可以使用範本檔案，來更輕鬆地自訂 HTML 內容送回。 這只是檢視的。

## <a name="adding-a-view-template"></a>新增檢視範本

若要使用檢視範本，我們將變更傳回 ActionResult，HomeController 索引方法，並讓它傳回 View()，例如，以下：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

上述變更表示而不是傳回字串，我們改為想要使用 「 檢視 」 來產生結果。

我們現在會將適當的檢視表範本加入專案中。 若要這樣做我們將定位文字內的資料指標的索引動作方法，然後以滑鼠右鍵按一下並選取 [新增檢視]。 這會顯示 [新增檢視] 對話方塊：

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

[新增檢視] 對話方塊可讓我們快速且輕鬆地產生檢視範本檔案。 根據預設 [新增檢視] 對話方塊會預先填入要使其符合使用它的動作方法所建立之檢視範本的名稱。 因為我們使用 [新增檢視] 內容功能表，我們 HomeController 的 index 動作方法內，上述的 [新增檢視] 對話方塊會預先填入預設的檢視名稱與具有 「 索引 」。 我們不需要變更任何選項，此對話方塊上，因此請按 [新增] 按鈕。

當我們按一下 [新增] 按鈕時，Visual Web Developer 會建立新的 Index.cshtml 檢視範本我們在 \Views\Home 目錄中，如果建立此資料夾不存在。

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml 」 檔案的名稱和資料夾位置是很重要，並會遵循預設 ASP.NET MVC 的命名慣例。 目錄名稱、 \Views\Home，符合的控制站-名為 HomeController。 檢視的範本名稱，索引中，符合控制器動作方法，這個方法會將所顯示的檢視。

ASP.NET MVC 可讓我們避免我們使用此命名慣例，傳回檢視時，明確指定的名稱或檢視表範本的位置。 它會根據預設轉譯 \Views\Home\Index.cshtml 檢視範本當我們我們 HomeController 內撰寫類似如下的程式碼：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer 中建立，並開啟 「 Index.cshtml 」 檢視範本之後我們按一下 [新增] 按鈕，在 [新增檢視] 對話方塊。 Index.cshtml 的內容如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

此檢視使用 Razor 語法，也就是更簡潔比 ASP.NET Web Form 和 ASP.NET MVC 的先前版本中使用 Web Form 檢視引擎。 Web Form 檢視引擎仍然可以使用在 ASP.NET MVC 3 中，但許多開發人員認為 Razor 檢視引擎會很適合開發 ASP.NET MVC。

前三個行設定使用 ViewBag.Title 頁面標題。 我們會查看其運作方式的詳細推出，但首先讓我們來更新文字標題文字，然後檢視頁面。 更新&lt;h2&gt;說出 「 這是 Home Page"，如下所示的標記。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

執行應用程式會顯示我們新的文字已顯示在 [首頁] 頁面上。

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>對於常見的站台元素使用的版面配置

大多數的網站有多個頁面之間共用的內容： 瀏覽、 頁尾、 標誌影像、 樣式表參考等。Razor 檢視引擎簡化這要使用一個稱為管理\_Layout.cshtml 自動/檢視表/Shared 資料夾內建立的。

![](mvc-music-store-part-3/_static/image4.png)

按兩下此資料夾來檢視內容，如下所示的。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

從我們的個別檢視的內容會由顯示@RenderBody（） 命令，以及任何通用的內容，我們想要出現在之外，可以加入至\_Layout.cshtml 標記。 我們希望我們首頁並儲存區的區域，在網站中，所有頁面上必須具有連結的通用標頭，因此我們會將其加入至上方的範本我們 MVC Music Store @RenderBody（） 陳述式。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>更新 樣式表

空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示驗證訊息的樣式。 某些其他 CSS 和圖像來定義外觀與風格，我們站台上，因此我們會將那些現在提供我們的設計工具。

更新 CSS 檔案和映像會包含在 MvcMusicStore Assets.zip，可在內容目錄[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。 我們將在 Windows 檔案總管中選取這兩種，放到我們的解決方案內容資料夾，在 Visual Web Developer 中，如下所示：

![](mvc-music-store-part-3/_static/image5.png)

系統會要求您確認是否您想要覆寫現有的 Site.css 檔案。 按一下 [是]。

![](mvc-music-store-part-3/_static/image6.png)

您的應用程式的內容資料夾現在會出現，如下所示：

![](mvc-music-store-part-3/_static/image7.png)

現在讓我們來執行應用程式，並查看我們的變更在首頁上的外觀。

![](mvc-music-store-part-3/_static/image8.png)

- 讓我們來檢視已變更的內容： 找到 HomeController 索引動作方法，並顯示這個 \Views\Home\Index.cshtmlView 範本時，即使我們的程式碼會呼叫 「 傳回 View()"，因為接下來我們檢視範本的標準命名慣例。
- 首頁會顯示簡單的歡迎訊息 \Views\Home\Index.cshtml 檢視範本內定義。
- [首頁] 使用我們\_Layout.cshtml 範本，因此歡迎訊息是否包含在標準網站 HTML 版面配置。

## <a name="using-a-model-to-pass-information-to-our-view"></a>使用模型來傳遞資訊給我們的檢視

只會顯示硬式編碼的 HTML 檢視範本不要讓非常有趣的網站。 若要建立動態網站，我們改為要從我們的控制器動作傳遞資訊至我們檢視範本。

在模型-檢視-控制器模式中，一詞是指的模型物件代表應用程式中的資料。 通常，模型物件對應至您的資料庫中的資料表，但它們不需要。

控制器動作方法會傳回 ActionResult 可以傳遞至檢視的模型物件。 這讓控制器能夠乾淨地封裝產生回應，，然後再的這項資訊傳遞至檢視表範本所需的所有資訊用來產生適當的 HTML 回應。 這是最容易了解藉由查看作用中，因此讓我們開始吧。

首先，我們將建立來代表我們的存放區中的內容類型和專輯某些模型類別。 首先建立內容類型的類別。 以滑鼠右鍵按一下您專案中的 「 模型 」 資料夾，選擇 [新增類別]，並將檔案"Genre.cs"。

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

然後加入所建立的類別公開的字串名稱屬性：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*注意： 在您想知道，案例 {get; 設定;} 標記法正在使用 C# 的自動實作屬性功能。這會提供屬性的優點而不需要我們宣告支援欄位。*

接下來，請遵循相同的步驟，建立具有標題和內容類型屬性的專輯類別 （名稱為 Album.cs）：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

現在我們可以修改 StoreController，若要使用檢視顯示我們的模型中的動態資訊。 如果-供示範之用現在-我們名為我們的要求識別碼為基礎的相簿，我們無法顯示與下列在檢視該資訊。

![](mvc-music-store-part-3/_static/image10.png)

我們會先變更存放區詳細資料動作，因此它會顯示單一專輯的資訊。 將"using"陳述式加入至頂端**StoreControllers**類別，以包含 MvcMusicStore.Models 命名空間中，所以我們不需要輸入 MvcMusicStore.Models.Album 每當我們想要使用的專輯類別。 該類別的"using"區段現在應該會顯示如下。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

接下來，我們將更新詳細資料控制器動作，使它傳回 ActionResult，而不是字串，以 HomeController 索引方法。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

現在我們可以修改專輯物件傳回至檢視的邏輯。 稍後在本教學課程中我們將會從資料庫擷取資料 –，但現在我們將使用 「 虛擬資料 「 若要開始使用。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*注意： 如果您熟悉 C#，您可能會假設，使用 var 表示我們專輯變數是晚期繫結。不正確-C# 編譯器使用類型推斷根據我們要指派給變數，以判斷該專輯屬於型別專輯和專輯本機變數編譯成專輯型別，因此我們會發生編譯時期檢查和 Visual Studio 程式碼編輯器支援。*

我們現在來建立使用我們的專輯產生 HTML 回應的檢視範本。 我們這樣做，我們需要先建置專案，加入檢視對話方塊知道我們新建的專輯類別。 您可以建置專案選取 Debug⇨Build MvcMusicStore 功能表項目 （額外的信用額度，您可以使用 Ctrl Shift B 捷徑來建置專案）。

![](mvc-music-store-part-3/_static/image11.png)

現在我們設定我們支援的類別，我們已準備好要建置我們檢視的範本。 以滑鼠右鍵按一下詳細資料的方法內，然後選取 [新增檢視] 從操作功能表。

![](mvc-music-store-part-3/_static/image12.png)

我們將建立新的檢視範本像我們之前使用 HomeController。 因為我們要建立它從 StoreController 它將會根據預設會產生 \Views\Store\Index.cshtml 檔案中。

不像之前，我們選取 [建立強型別] 檢視的核取方塊。 然後我們選取 [檢視資料的類別] 卸除 downlist 內我們"專輯 」 類別。 這會導致建立檢視範本所預期的相簿，物件會傳遞，以便使用 [新增檢視] 對話方塊。

![](mvc-music-store-part-3/_static/image13.png)

我們按一下 [新增] 按鈕將會建立包含下列程式碼我們 \Views\Store\Details.cshtml 檢視範本。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

請注意第一行中，這表示此檢視為強型別到專輯類別。 Razor 檢視引擎了解，它已經通過專輯物件，因此我們可以輕鬆地存取模型屬性，即使已在 Visual Web Developer 編輯器中 IntelliSense 的優點。

更新&lt;h2&gt;標記，讓它將顯示的專輯標題屬性修改該一行出現，如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

請注意當您輸入超過此時限後的，會觸發 IntelliSense@Model關鍵字，顯示的屬性和專輯類別支援的方法。

我們現在重新執行受測專案，並瀏覽市集/詳細資料/5 URL。 我們會看到類似下方的相簿的詳細資料。

![](mvc-music-store-part-3/_static/image14.png)

現在我們要在存放區瀏覽動作方法的類似更新。 Update 方法，以便傳回 ActionResult，並修改方法邏輯，以便建立新的內容類型物件並傳回至檢視。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

以滑鼠右鍵按一下 瀏覽方法中，選取 新增檢視 從內容功能表，然後新增是強型別檢視的強型別加入內容類型的類別。

![](mvc-music-store-part-3/_static/image15.png)

更新&lt;h2&gt;檢視中的項目中的程式碼 （/Views/Store/Browse.cshtml) 以顯示類型資訊。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

現在讓我們重新執行受測專案，並瀏覽至存放區/瀏覽？內容類型 = Disco URL。 我們會看到顯示類似如下的 [瀏覽] 頁面。

![](mvc-music-store-part-3/_static/image16.png)

最後，讓我們進行稍微更為複雜的更新**存放區索引**動作方法並檢視我們的存放區中顯示的所有內容類型清單。 我們會使用執行此動作清單的內容類型為我們的模型物件，而不只單一內容類型。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

存放區索引動作方法中以滑鼠右鍵按一下，然後選取 新增檢視之前，請選取內容類型為模型類別，然後按 新增 按鈕。

![](mvc-music-store-part-3/_static/image17.png)

我們將會在第一次變更@model而不是其中一個物件，表示檢視將會需要數個內容類型的宣告。 變更 /Store/Index.cshtml 讀取，如下所示的第一行：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

這會告訴 Razor 檢視引擎將會使用可以保存數個內容類型物件的模型物件。 我們會使用 IEnumerable&lt;類型&gt;而不是清單&lt;類型&gt;因為這是一般，讓我們將稍後我們模型型別變更為支援 IEnumerable 介面的物件類型。

接下來，我們會執行迴圈內容類型中的物件模型，以下的檢視已完成程式碼所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

請注意，我們有完整的 IntelliSense 支援當我們輸入這個程式碼，如此當我們輸入"@Model。 」 我們會看到所有方法和屬性支援的型別內容類型的 IEnumerable。

![](mvc-music-store-part-3/_static/image18.png)

"Foreach"迴圈，在 Visual Web Developer 會知道的每個項目屬於型別內容類型，因此我們會看到 IntelliSence 每個內容類型的型別。

![](mvc-music-store-part-3/_static/image19.png)

接著，scaffolding 功能檢查的內容類型的物件，並決定，每個都會有名稱屬性，以便執行迴圈，並將其輸出。它也會產生每個個別項目的編輯、 詳細資料及刪除連結。 我們將利用，稍後我們存放區管理員 中，但現在我們想要改為有一個簡單的清單。

當我們執行應用程式，並瀏覽至 /Store 時，我們看到計數和內容類型的清單會顯示。

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>新增頁之間的連結

列出目前的內容類型我們 /Store URL 只是以純文字格式列出內容類型名稱。 讓我們來變更這個值，而不是純文字，我們改為需要內容類型名稱連結至適當的存放區/瀏覽 URL，以便讓按一下像是"Disco"會瀏覽至存放區/瀏覽音樂內容類型？ 內容類型 = Disco URL。 我們無法更新使用這些連結的程式碼下方的輸出我們 \Views\Store\Index.cshtml 檢視範本**（不要輸入下列命令中-我們改善）**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

此作業有效，但它可能會導致稍後疑難排解，因為它是倚賴硬式編碼字串。 比方說，如果我們想要重新命名控制器，我們需要搜尋尋找需要更新的連結的程式碼。

我們可以使用的替代方式是利用 HTML Helper 方法。 ASP.NET MVC 包括可從檢視範本程式碼來執行各種不同的一般工作，就像這樣的 HTML Helper 方法。 Html.ActionLink() helper 方法是一個特別有用，可讓您更輕鬆地建立 HTML &lt;&gt;連結，並負責造成困擾的詳細資料，想要確定具有正確的 URL 路徑 URL 編碼。

Html.ActionLink() 有數個不同的多載可讓您指定所需的連結資訊。 在最簡單的情況下，您會提供連結文字和用戶端上按下超連結時，移至動作方法。 例如，我們可以連結至 「 / 存放區 /"index （） 方法，在存放區詳細資料 頁面上，以連結文字 「 移至存放區索引 」 中使用下列呼叫：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*注意： 在此情況下，我們不需要指定控制器名稱，因為我們只連結到相同的控制站，會呈現目前檢視中的另一個動作。*

我們瀏覽 頁面的連結需要傳遞參數，不過，因此我們將使用另一個方法多載，Html.ActionLink 採用三個參數：

- 1. 連結文字，將會顯示類型名稱
- 2. 控制器的動作名稱 （瀏覽）
- 3. 路由參數值，指定的名稱 （類型） 和值 （類型名稱）

將放，同時，以下我們要如何撰寫這些連結到存放區索引檢視：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

現在我們再次執行受測專案，而且存取 /Store/ URL 時，我們會看到內容類型的清單。 按下時，每個內容類型會是超連結 – 花我們我們/存放區/瀏覽？ 內容類型 =*[類型]* URL。

![](mvc-music-store-part-3/_static/image3.jpg)

內容類型清單的 HTML 看起來像這樣：

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
[上一頁](mvc-music-store-part-2.md)
[下一頁](mvc-music-store-part-4.md)
