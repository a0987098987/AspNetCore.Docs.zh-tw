---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: 新增檢視 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7c7179419a126eaa998252e75ac38b3b1a2b23d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830977"
---
<a name="adding-a-view"></a>新增檢視
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本節中您要修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。

您將建立檢視範本檔案中使用[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)使用 ASP.NET MVC 3 引進。 以 razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供優雅的方式來建立 HTML 輸出使用 C#。 Razor 字元和時撰寫檢視範本時，所需按鍵的數目降至最低，並可讓程式碼撰寫工作流程迅速、 流暢。

目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。 變更`Index`方法來傳回`View`物件，如下列程式碼所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上述方法來產生 HTML 回應給瀏覽器中使用檢視範本。 控制器方法 (也稱為[動作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，例如`Index`上述，方法通常會傳回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或衍生自類別[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不是基本類型與字串相同。

在專案中，新增您可以使用檢視範本`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設值的方式，然後按一下 **新增**按鈕：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。 您可以看到它們**方案總管 中**:

![](adding-a-view/_static/image3.png)

下列所示*Index.cshtml*所建立的檔案：

![HelloWorldIndex](adding-a-view/_static/image4.png)

新增下列 HTML 下的`<h2>`標記。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完整*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

如果您使用 Visual Studio 2012 中，在 [方案總管] 中，以滑鼠右鍵按一下*Index.cshtml*檔案，然後選取**Page Inspector 中的檢視**。

![PI](adding-a-view/_static/image5.png)

[Page Inspector 教學課程](../../views/using-page-inspector-in-aspnet-mvc.md)提供的這項新工具的詳細資訊。

或者，執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，其所指定方法應使用檢視範本檔案來呈現至瀏覽器的回應。 因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*檢視中的檔案*\Views\HelloWorld*資料夾。 下圖顯示的字串&quot;Hello from our View Template ！&quot;硬式編碼在檢視中。

![](adding-a-view/_static/image6.png)

看起來很好。 不過，請注意，瀏覽器的標題列會顯示&quot;我的 ASP.NET 的編製索引&quot;並在頁面頂端的大型連結會顯示&quot;您的標誌。&quot;下面&quot;您標誌。&quot;連結大約會註冊記錄檔中的連結，並連結至首頁，以下和連絡人 頁面。 讓我們變更其中某些問題。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，您想要變更的地方&quot;您標誌。&quot;標題在頁面頂端。 該文字會每個頁面。 雖然看起來的應用程式中的每個頁面上，它實際上被實作在專案中，只有一個位置。 移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.cshtml*檔案。 這個檔案稱為*版面配置頁*很共用&quot;shell&quot;所有其他頁面使用。

![_LayoutCshtml](adding-a-view/_static/image7.png)

版面配置範本可讓您在一個位置指定的 HTML 容器配置，您的網站，然後將它套用到您的網站中的多個頁面。 找到 `@RenderBody()` 這行。 `RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」&quot;&quot;在版面配置頁中。 例如，如果您選取 [關於] 連結中， *Views\Home\About.cshtml*內呈現檢視`RenderBody`方法。

變更網站標題中的 標題的版面配置範本，從&quot;您的標誌&quot;要&quot;MVC Movie&quot;。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

取代為下列標記中的標題項目的內容：

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

執行應用程式和通知，現在指出&quot;MVC Movie &quot;。 按一下 **關於**連結，您會看到該頁面會顯示如何&quot;MVC Movie&quot;也。 我們能夠在版面配置範本一次進行變更，並已在站台上的所有頁面都反映新的標題。

![](adding-a-view/_static/image8.png)

現在，讓我們變更索引 檢視的標題。

開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。 有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。 您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

若要表示顯示，上述程式碼的 HTML 標題`Title`的屬性`ViewBag`物件 (這是在*Index.cshtml*檢視範本)。 如果您回頭看的版面配置範本的原始程式碼中，您會注意到的範本會使用此值`<title>`一部分的項目`<head>`我們先前修改 HTML 一節。 使用此`ViewBag`方法時，您可以輕鬆地傳遞其他參數之間檢視範本和配置檔案。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題會透過`ViewBag.Title`我們在中設定*Index.cshtml*檢視範本和其他&quot;-Movie App&quot;配置檔案中新增。

另外也請注意如何在內容*Index.cshtml*檢視範本與合併 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。

![](adding-a-view/_static/image9.png)

我們一點&quot;資料&quot;(在此情況下&quot;Hello from our View Template ！&quot;訊息) 是硬式編碼，不過。 MVC 應用程式具有&quot;V&quot; （檢視），您可盡情&quot;C&quot; （控制器），但沒有&quot;M&quot; （模型） 尚未。 不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。 控制器類別會叫用，以回應傳入的 URL 要求。 控制器類別是回應的您可在此撰寫處理傳入的瀏覽器的程式碼要求、 從資料庫擷取資料和最終決定哪一種將傳回給瀏覽器。 然後從控制器來產生並格式化瀏覽器的 HTML 回應使用檢視範本。

控制器是負責提供任何資料或物件所需為了讓檢視範本來呈現至瀏覽器的回應。 最佳做法是：**檢視範本應該永遠不會執行商務邏輯或直接與資料庫互動**。 相反地，檢視範本應該只使用由控制器提供給它的資料。 維護這&quot;關注點分離&quot;精簡、 可測試且更易於維護，協助保持您的程式碼。

目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出到瀏覽器直接的值。 與其讓控制器呈現這個回應，做為字串，讓我們變更控制器以改為使用檢視範本。 檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。 您可以透過讓控制器將檢視範本需要的動態資料 （參數） 放置`ViewBag`檢視範本之後可以存取的物件。

返回*HelloWorldController.cs*檔案，並變更`Welcome`方法來加入`Message`並`NumTimes`值`ViewBag`物件。 `ViewBag` 是動態物件，這表示您可以放置任何內容給它;`ViewBag`物件有定義的屬性，直到您將其內的項目。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應的具名的參數 (`name`和`numTimes`) 從 [網址] 列到您的方法中的參數中的查詢字串。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

現在`ViewBag`物件包含將會自動傳遞至檢視的資料。

接下來，您需要一個歡迎視圖模板！ 在 **建置**功能表上，選取**建置 MvcMovie**藉此確定編譯專案。

然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image10.png)

以下是什麼**加入檢視**對話方塊看起來像：

![](adding-a-view/_static/image11.png)

按一下 **新增**，然後新增下列程式碼底下`<h2>`在新的項目*Welcome.cshtml*檔案。 您會建立迴圈，指出&quot;Hello&quot;使用者講的是預期的次數。 完整*Welcome.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

執行應用程式，並瀏覽至下列 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

資料是從 URL 取得並傳遞控制站使用現在[模型繫結器](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。 控制器將資料封裝成`ViewBag`物件並檢視該物件傳遞。 檢視則會顯示為 HTML 資料給使用者。

![](adding-a-view/_static/image12.png)

在上述範例中，我們使用`ViewBag`將資料從控制器傳遞至檢視的物件。 第二個教學課程中，我們將檢視模型將資料從控制器傳遞至檢視。 若要將資料傳遞的檢視模型方法通常會更優先檢視包方法。 請參閱部落格文章[動態 V 強型別檢視](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)如需詳細資訊。

其實是一種的&quot;M&quot;模型，但不是資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
