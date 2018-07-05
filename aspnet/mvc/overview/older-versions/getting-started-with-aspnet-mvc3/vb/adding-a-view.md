---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: 新增檢視 (VB) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 840f925aea40963c45787a8b5af20e99ff31f3d8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386782"
---
<a name="adding-a-view-vb"></a>新增檢視 (VB)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 VB.NET 原始程式碼的 Visual Web Developer 專案。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-a-view.md)本教學課程。


這一節我們將修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。

讓我們開始使用檢視範本，內含`Index`方法中的`HelloWorldController`類別。 目前`Index`方法會傳回硬式編碼在控制器類別內的訊息字串。 變更`Index`方法來傳回`View`物件，如下列所示：

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

讓我們現在將檢視範本新增至我們的專案，我們可以叫用`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設的項目，然後按一下**新增** 按鈕。

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.vbhtml*建立檔案。 您可以看到它們**方案總管 中**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

將下的一些 HTML`<h2>`標記。 已修改*MvcMovie\Views\HelloWorld\Index.vbhtml*檔案如下所示。

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

執行應用程式，並瀏覽至&quot;hello world&quot;控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，這表示我們想要使用檢視範本檔案來呈現給用戶端的回應。 因為我們未明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.vbhtml*檢視檔案內*\Views\HelloWorld*資料夾。 下圖顯示字串硬式編碼在檢視中。

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

看起來很好。 不過，請留意到瀏覽器的標題列顯示&quot;Index&quot; ，並大的標題在頁面上顯示&quot;我的 MVC 應用程式。&quot;讓我們將這些變更。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，讓我們變更文字&quot;我的 MVC 應用程式。&quot;該文字會共用，並出現在每一頁。 它實際上不會顯示只有一個位置，在專案中，即使是在我們的應用程式中的每個頁面上。 移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.vbhtml*檔案。 這個檔案稱為版面配置頁面，而且共用&quot;shell&quot;所有其他頁面使用。

請注意`@RenderBody()`檔案底部附近的程式碼行。 `RenderBody` 是，您所建立的所有頁面的都顯示位置的預留位置&quot;包裝&quot;版面配置頁。 變更`<h1>`標題從**&quot;** 我的 MVC 應用程式&quot;來&quot;MVC 電影應用程式&quot;。

[!code-html[Main](adding-a-view/samples/sample3.html)]

執行應用程式，並記下它現在說&quot;MVC 電影應用程式&quot;。 按一下 **關於**連結，且頁面上顯示&quot;MVC 電影應用程式&quot;也。

完整 *\_Layout.vbhtml*檔案如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

現在，讓我們變更 （檢視） 中的 [索引] 頁面的標題。

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

開啟*MvcMovie\Views\HelloWorld\Index.vbhtml*。 有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。 我們要讓它們稍有不同的位元的程式碼變更應用程式的一部分，您可以看到。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 它很容易就能只進行小變更的應用程式中的重大變更對檢視表。 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

我們一點&quot;資料&quot;(在此情況下&quot;Hello World ！&quot;訊息) 是硬式編碼，不過。 MVC 應用程式可能會有 V （檢視），我們有 C （控制站），但沒有 M （模型） 尚未。 不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。 我們想要將檢視範本呈現 HTML 回應給用戶端所需要的傳遞。 這些物件是通常建立控制器類別傳遞至檢視範本，而且它們應該包含在檢視範本需要的資料，並不多。

與先前`HelloWorldController`類別，`Welcome`動作方法所花費`name`和`numTimes`參數和參數值至瀏覽器然後輸出。 而非讓控制器繼續直接呈現這個回應，讓我們改為我們會將放該資料封包中檢視。 控制器和檢視，可以使用`ViewBag`物件來保存該資料。 會自動傳遞到檢視範本，以及用來呈現 HTML 回應使用封包的內容做為資料。 這樣一來，控制器會關心一件事並與另一個檢視範本，讓我們可以維護乾淨&quot;關注點分離&quot;應用程式中。

或者，我們可以定義的自訂類別，然後建立該物件的執行個體自己、 填入資料並將它傳遞至檢視。 通常稱為 ViewModel，因為它是自訂模型檢視。 若是小量的資料，不過，ViewBag 運作良好。

返回*HelloWorldController.vb*檔案變更`Welcome`NumTimes 與訊息放入 ViewBag 控制器內的方法。 ViewBag 是動態物件。 這表示您可以放置任何內容給它。 ViewBag 有定義的屬性，直到您將其內的項目。

完整`HelloWorldController.vb`與新的類別，在相同的檔案。

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

現在我們 ViewBag 包含將自動傳遞到檢視的資料。 同樣地，或者我們可以具有傳入我們自己的物件，就像這樣如果我們想：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

現在，我們必須`WelcomeView`範本 ！ 執行應用程式，因此新程式碼會編譯。 關閉瀏覽器，以滑鼠右鍵按一下`Welcome`方法，然後再按一下**加入檢視**。

以下是您**加入檢視**對話方塊看起來像。

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

下列程式碼下新增`<h2>`中新項目<em>褖畫惎。</em>vbhtml 檔案。 我們將進行迴圈，並假設&quot;Hello&quot;使用者說我們應該無限次 ！

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

執行應用程式，並瀏覽至 `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

現在資料已從 URL 取得，會自動傳遞到控制器。 控制器會封裝將資料分成`Model`物件並檢視該物件傳遞。 檢視比對使用者顯示為 HTML 的資料。

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

其實是一種的&quot;M&quot;模型，但不是資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
