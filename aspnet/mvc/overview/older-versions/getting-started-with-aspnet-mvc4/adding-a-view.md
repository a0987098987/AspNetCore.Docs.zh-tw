---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: 加入檢視 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 61a93c1430e9e39543c69b84901a50ceb710a5ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view"></a>加入的檢視
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


本節中您要修改`HelloWorldController`類別若要使用範本檔案來完全封裝產生 HTML 回應至用戶端的程序的檢視。

您將建立檢視範本檔案使用[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)所導入的 ASP.NET MVC 3。 Razor 為基礎的檢視範本具有*.cshtml*副檔名，且提供簡潔的方式建立 HTML 輸出使用 C#。 Razor 的字元和撰寫檢視範本時所需按鍵數目降到最低，並可讓快速，流體，程式碼撰寫工作流程。

目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。 變更`Index`方法以傳回`View`物件，如下列程式碼所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上述方法，使用檢視範本來產生瀏覽器的 HTML 回應。 控制器方法 (也稱為[動作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，例如`Index`上述，方法通常會傳回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或類別衍生自[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不是基本型別類似字串。

在專案中，加入您可以使用檢視範本`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設值的方式，然後按一下 **新增**按鈕：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*資料夾和*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。 您可以看到它們在**方案總管 中**:

![](adding-a-view/_static/image3.png)

下列示範*Index.cshtml*所建立的檔案：

![HelloWorldIndex](adding-a-view/_static/image4.png)

加入下的下列 HTML`<h2>`標記。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完整*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

如果您使用 Visual Studio 2012，在 [方案總管] 中，以滑鼠右鍵按一下*Index.cshtml*檔案，然後選取**Page Inspector 中的檢視**。

![PI](adding-a-view/_static/image5.png)

[Page Inspector 教學課程](../../views/using-page-inspector-in-aspnet-mvc.md)的這項新工具的詳細資訊。

或者，執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法不執行許多工作; 它只需執行陳述式`return View()`，其中指定的方法來呈現瀏覽器的回應，使用檢視範本檔案。 因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*中的檢視檔案*\Views\HelloWorld*資料夾。 下圖顯示字串&quot;我們檢視範本中的 Hello ！&quot;硬式編碼在檢視中。

![](adding-a-view/_static/image6.png)

看起來很好。 不過，請注意，在瀏覽器標題列顯示&quot;索引我的 ASP.NET A&quot;大連結的頁面頂端顯示 &quot;您的標誌。&quot;下面&quot;您標誌。&quot;連結大約會註冊記錄檔的連結，或其下，首頁連結，並連絡頁面。 我們來變更這些名稱。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，您想要變更&quot;您標誌。&quot;在頁面頂端的標題。 該文字會每一頁。 雖然會顯示應用程式中的每一頁上，它實際上被實作在專案中，只有一個位置。 移至*/檢視表/共用*資料夾中的**方案總管 中**並開啟 *\_Layout.cshtml*檔案。 這個檔案稱為*版面配置頁*而且它是共用&quot;殼層&quot;所有其他頁面使用。

![_LayoutCshtml](adding-a-view/_static/image7.png)

版面配置範本可讓您指定 HTML 容器的配置，您的網站在同一個地方，然後將它套用到您的網站中的多個頁面。 找到 `@RenderBody()` 這行。 `RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」&quot;&quot;在版面配置頁中。 例如，如果您選取 [關於] 連結， *Views\Home\About.cshtml*內呈現檢視`RenderBody`方法。

變更網站標題中的標題中的版面配置範本&quot;您的標誌&quot;至&quot;MVC 影片&quot;。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

以下列標記取代標題項目的內容：

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

執行應用程式和注意，現在顯示&quot;MVC 影片&quot;。 按一下**有關**連結，而且您會看到該頁面會顯示如何&quot;MVC 影片&quot;、 太。 我們能夠在版面配置範本的一次進行變更，並已在網站上的所有頁面都反映新的標題。

![](adding-a-view/_static/image8.png)

現在，讓我們來變更索引檢視的標題。

開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。 若要變更的兩個地方： 第一次，文字會出現在標題的瀏覽器，然後在次要標頭 (`<h2>`項目)。 您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

若要表示要顯示，請設定上方的程式碼的 HTML 標題`Title`屬性`ViewBag`物件 (它是*Index.cshtml*檢視範本)。 如果您查看上一步的版面配置範本的原始程式碼，您會發現，範本會使用這個值`<title>`一部分的項目`<head>`> 一節，我們在先前所修改的 html。 使用此`ViewBag`方法時，您可以輕鬆地傳遞其他參數檢視範本之間配置檔案。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題會透過`ViewBag.Title`我們中設定*Index.cshtml*檢視範本和其他&quot;-電影應用程式&quot;加入版面配置的檔案。

同時也請注意如何在內容*Index.cshtml*檢視範本與合併，而 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。

![](adding-a-view/_static/image9.png)

我們一點&quot;資料&quot;(在此情況下&quot;我們檢視範本中的 Hello ！&quot;訊息) 是硬式編碼，不過。 在 MVC 應用程式具有&quot;V&quot; （檢視） 和你&quot;C&quot; （控制站），但沒有&quot;M&quot; （模型） 尚未。 稍後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，我們要先討論有關將資訊從控制器傳遞至檢視。 控制器類別會叫用，以回應傳入的 URL 要求。 控制器類別是回應的您撰寫處理內送的瀏覽器的程式碼要求、 從資料庫擷取資料和最終決定何種瀏覽器中將傳回。 然後從控制器來產生並格式化 HTML 回應至瀏覽器使用檢視範本。

控制站會負責提供任何資料或物件必須轉譯至瀏覽器的回應的檢視範本的順序。 最佳作法：**檢視範本應該永遠不會執行商務邏輯或直接與資料庫互動**。 相反地，檢視範本應該僅適用於由控制器提供給它的資料。 維護這&quot;的重要性分離&quot;全新、 可測試且更易於維護，可協助保持您程式碼。

目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出至瀏覽器直接的值。 而非保有呈現為字串的這個回應的控制站，我們來變更要改為使用檢視範本的控制站。 檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。 您可以透過將檢視需要的範本中的動態資料 （參數） 的控制站`ViewBag`檢視範本可以存取的物件。

返回*HelloWorldController.cs*檔案，並且變更`Welcome`方法，將`Message`和`NumTimes`值設定為`ViewBag`物件。 `ViewBag` 是動態的物件，這表示您可以將您所要的任何。`ViewBag`物件沒有任何定義的屬性，直到您將放在它之內的項目。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應的具名的參數 (`name`和`numTimes`) 從您的方法中參數的 [網址] 列中的查詢字串。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

現在`ViewBag`物件包含會自動傳遞到檢視的資料。

接下來，您需要  褖畫惎檢視範本 ！ 在**建置**功能表上，選取**建置 MvcMovie**確定編譯專案。

然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image10.png)

以下是**加入檢視**對話方塊看起來像：

![](adding-a-view/_static/image11.png)

按一下**新增**，然後加入下列程式碼`<h2>`中新項目*Welcome.cshtml*檔案。 您會建立迴圈，該處會指示&quot;Hello&quot; ，使用者可指定它應該的次數。 完整*Welcome.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

執行應用程式，並瀏覽至下列 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

資料現在是取自 URL，並傳遞至控制器使用[模型繫結器](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。 控制器封裝資料`ViewBag`物件，以及檢視該物件傳遞。 檢視然後會顯示為 HTML 資料給使用者。

![](adding-a-view/_static/image12.png)

在上述範例中，我們使用`ViewBag`將控制器中的資料傳遞到檢視的物件。 後者教學課程中，我們將使用檢視模型傳遞資料從控制器到檢視。 傳遞資料的檢視模型方法通常合用透過。 檢視包方法 請參閱部落格文章：[動態 V 強型別檢視](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)如需詳細資訊。

就是一種的&quot;M&quot;的模型，但資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
