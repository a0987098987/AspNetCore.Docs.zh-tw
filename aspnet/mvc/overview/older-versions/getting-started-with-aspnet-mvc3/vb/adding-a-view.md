---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: 加入的檢視 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-vb"></a>加入的檢視 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-a-view.md)本教學課程。


這一節我們將修改`HelloWorldController`類別來使用檢視範本檔案完全封裝的程序產生 HTML 用戶端的回應。

讓我們開始使用檢視範本與`Index`方法中的`HelloWorldController`類別。 目前`Index`方法會傳回硬式編碼控制器類別內的訊息字串。 變更`Index`方法以傳回`View`物件，如下所示：

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

我們現在將檢視範本加入至受測專案，我們可以叫用`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設的項目，再按**新增** 按鈕。

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld*資料夾和*MvcMovie\Views\HelloWorld\Index.vbhtml*建立檔案。 您可以看到它們在**方案總管 中**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

將一些 HTML 下的`<h2>`標記。 修改*MvcMovie\Views\HelloWorld\Index.vbhtml*檔案如下所示。

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

執行應用程式，並瀏覽至&quot;hello world&quot;控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法不執行許多工作; 它只需執行陳述式`return View()`，這表示我們想要使用的檢視範本檔案來呈現給用戶端的回應。 因為我們並未明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.vbhtml*內的檢視檔案*\Views\HelloWorld*資料夾。 下圖顯示字串硬式編碼在檢視中。

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

看起來很好。 不過，請注意，在瀏覽器標題列顯示&quot;索引&quot;大的標題，在頁面上顯示 &quot;我的 MVC 應用程式。&quot;讓我們將這些變更。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，我們變更文字&quot;我的 MVC 應用程式。&quot;該文字共用，並顯示每一頁上。 它實際上不會顯示僅在一個地方我們的受測專案，即使它是我們的應用程式中的每一頁上。 移至*/檢視表/共用*資料夾中的**方案總管 中**並開啟 *\_Layout.vbhtml*檔案。 這個檔案稱為版面配置頁面，它是共用&quot;殼層&quot;所有其他頁面使用。

請注意`@RenderBody()`檔案底部附近的程式碼行。 `RenderBody` 這是的預留位置，其中您所建立的所有頁面會都出現&quot;包裝&quot;版面配置頁面中。 變更`<h1>`標題從**&quot;**我的 MVC 應用程式&quot;至&quot;MVC 電影應用程式&quot;。

[!code-html[Main](adding-a-view/samples/sample3.html)]

執行應用程式，並注意它現在說&quot;MVC 電影應用程式&quot;。 按一下**有關**連結，且頁面上顯示&quot;MVC 電影應用程式&quot;、 太。

完整 *\_Layout.vbhtml*檔案如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

現在，我們來變更索引頁面 （檢視） 的標題。

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

開啟*MvcMovie\Views\HelloWorld\Index.vbhtml*。 若要變更的兩個地方： 第一次，文字會出現在標題的瀏覽器，然後在次要標頭 (`<h2>`項目)。 我們會讓它們稍有不同，才可看到的許多程式碼變更的應用程式的一部分。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 所以可以輕鬆地變更大些微變更，您的應用程式中檢視。 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

我們一點&quot;資料&quot;(在此情況下&quot;Hello World ！&quot;訊息) 是硬式編碼，不過。 我們的 MVC 應用程式具有 V (views)，而且我們有 C （控制站），但沒有 M （模型） 尚未。 稍後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，我們要先討論有關將資訊從控制器傳遞至檢視。 我們想要傳遞呈現 HTML 回應至用戶端所需要的檢視範本。 通常建立和由控制器類別傳遞給檢視範本時，這些物件，而且它們應該包含檢視範本需要的資料，並不多。

先前使用`HelloWorldController`類別`Welcome`動作方法耗用`name`和`numTimes`參數和參數值至瀏覽器然後輸出。 而非保有繼續直接轉譯這個回應的控制站，讓我們改為說該資料包中檢視。 控制器和檢視可以使用`ViewBag`物件來保存該資料。 以便自動傳遞到檢視表範本，並且用來呈現 HTML 回應使用的屬性包內容做為資料。 控制器關於一件事，而另一個檢視範本，這樣一來，讓我們能夠維護全新&quot;的重要性分離&quot;應用程式中。

或者，我們無法定義自訂類別，然後自己上建立該物件的執行個體、 填入資料再將它傳遞至檢視。 通常稱為 ViewModel，因為它是自訂的檢視模型。 對於小量的資料，不過，ViewBag 非常適合。

返回*HelloWorldController.vb*檔案變更`Welcome`內將訊息和 NumTimes 放入 ViewBag 控制器方法。 ViewBag 是動態的物件。 這表示您可以將任何您要它。 ViewBag 並沒有已定義的內容，直到您將放在它之內的項目。

完整`HelloWorldController.vb`與新的類別，在相同的檔案。

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

現在我們 ViewBag 包含會自動傳遞到檢視的資料。 同樣地，或者我們可以有自己的物件，像這樣如果傳入我們喜歡：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

現在，我們必須`WelcomeView`範本 ！ 執行應用程式，因此新的程式碼會編譯。 關閉瀏覽器，以滑鼠右鍵按一下`Welcome`方法，然後再按一下**加入檢視**。

以下是您**加入檢視**對話方塊看起來。

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

下列程式碼下新增`<h2>`中新項目<em> 褖畫惎。</em>vbhtml 檔案。 我們將進行迴圈，並假設&quot;Hello&quot;依需求多次使用者可指定我們應該 ！

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

執行應用程式，並瀏覽至 `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

現在從 URL 取得及資料自動傳送到控制器。 控制器的封裝將資料`Model`物件，以及檢視該物件傳遞。 檢視比對使用者顯示為 HTML 的資料。

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

就是一種的&quot;M&quot;的模型，但資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
