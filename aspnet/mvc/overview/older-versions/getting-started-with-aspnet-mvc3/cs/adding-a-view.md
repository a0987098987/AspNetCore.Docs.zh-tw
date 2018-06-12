---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: 加入的檢視 (C#) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873166"
---
<a name="adding-a-view-c"></a>加入的檢視 (C#)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。
> 
> 
> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。 [下載的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換至[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


本節中您要修改`HelloWorldController`類別若要使用範本檔案來完全封裝產生 HTML 回應至用戶端的程序的檢視。

您將建立使用新的檢視範本檔案[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)所導入的 ASP.NET MVC 3。 Razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供簡潔的方式建立 HTML 輸出使用 C#。 Razor 的字元和撰寫檢視範本時所需按鍵數目降到最低，並可讓快速，流體，程式碼撰寫工作流程。

開始使用檢視範本與`Index`方法中的`HelloWorldController`類別。 目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。 變更`Index`方法以傳回`View`物件，如下所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

這個程式碼中使用檢視範本產生瀏覽器的 HTML 回應。 在專案中，加入您可以使用檢視範本`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設值的方式，然後按一下 **新增**按鈕：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*資料夾和*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。 您可以看到它們在**方案總管 中**:

![](adding-a-view/_static/image3.png)

下列示範*Index.cshtml*所建立的檔案：

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

將一些 HTML 下的`<h2>`標記。 修改*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法不執行許多工作; 它只需執行陳述式`return View()`，其中指定的方法來呈現瀏覽器的回應，使用檢視範本檔案。 因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*中的檢視檔案*\Views\HelloWorld*資料夾。 下圖顯示字串硬式編碼在檢視中。

![](adding-a-view/_static/image6.png)

看起來很好。 不過，請注意，在瀏覽器標題列會顯示"Index"大的標題，在頁面上顯示 「 我 MVC 應用程式。 」 讓我們將這些變更。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，您會想要變更在頁面頂端的 「 我的 MVC 應用程式 」 標題。 該文字會每一頁。 它實際上被實作僅在一個地方，在專案中，雖然會顯示應用程式中的每一頁上。 移至 */檢視表/共用*資料夾中的**方案總管 中**並開啟 *\_Layout.cshtml*檔案。 這個檔案稱為*版面配置頁*而且它是共用 「 殼層 」 的所有其他頁面使用。

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

版面配置範本可讓您指定 HTML 容器的配置，您的網站在同一個地方，然後將它套用到您的網站中的多個頁面。 請注意`@RenderBody()`檔案底部附近的行。 `RenderBody` 這是的預留位置，其中您所建立的所有檢視特定頁面會都出現 「 包裝 」 中之版面配置頁。 變更從 「 我 MVC 應用程式 」 到 「 MVC 電影應用程式 」 的版面配置範本中的標題。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

執行應用程式，並注意它現在說 「 MVC 電影應用程式 」。 按一下**有關**連結，而且您會看到如何該頁面會顯示"MVC 影片 App"太。 我們能夠在版面配置範本的一次進行變更，並已在網站上的所有頁面都反映新的標題。

![](adding-a-view/_static/image9.png)

完整 *\_Layout.cshtml*檔案如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

現在，我們來變更索引頁面 （檢視） 的標題。

開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。 若要變更的兩個地方： 第一次，文字會出現在標題的瀏覽器，然後在次要標頭 (`<h2>`項目)。 您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

若要表示要顯示，請設定上方的程式碼的 HTML 標題`Title`屬性`ViewBag`物件 (它是*Index.cshtml*檢視範本)。 如果您查看上一步的版面配置範本的原始程式碼，您會發現，範本會使用這個值`<title>`一部分的項目`<head>`> 一節的 html。 使用這個方法，可以檢視範本與版面配置檔案之間輕鬆地傳遞其他參數。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。

同時也請注意如何在內容*Index.cshtml*檢視範本與合併，而 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。

![](adding-a-view/_static/image10.png)

然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!" 訊息) 是硬式編碼的資料。 MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。 稍後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，我們要先討論有關將資訊從控制器傳遞至檢視。 控制器類別會叫用，以回應傳入的 URL 要求。 控制器類別是回應的您撰寫的程式碼會處理傳入的參數、 從資料庫擷取資料並最終決定傳送回瀏覽器類型的地方。 然後從控制器來產生並格式化 HTML 回應至瀏覽器使用檢視範本。

控制站會負責提供任何資料或物件必須轉譯至瀏覽器的回應的檢視範本的順序。 檢視範本應該永遠不會執行商務邏輯，或直接與資料庫互動。 相反地，它應該僅適用於由控制器提供給它的資料。 維護此 「 重要性分離 」，可協助確保您的程式碼，全新且更易於維護。

目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出至瀏覽器直接的值。 而非保有呈現為字串的這個回應的控制站，我們來變更要改為使用檢視範本的控制站。 檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。 您可以透過將動態檢視需要的範本中的資料放在控制器`ViewBag`檢視範本可以存取的物件。

返回*HelloWorldController.cs*檔案，並且變更`Welcome`方法，將`Message`和`NumTimes`值設定為`ViewBag`物件。 `ViewBag` 是動態的物件，這表示您可以將您所要的任何。`ViewBag`物件沒有任何定義的屬性，直到您將放在它之內的項目。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

現在`ViewBag`物件包含會自動傳遞到檢視的資料。

接下來，您需要  褖畫惎檢視範本 ！ 在**偵錯**功能表上，選取**建置 MvcMovie**確定編譯專案。

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。 以下是**加入檢視**對話方塊看起來像：

![](adding-a-view/_static/image13.png)

按一下**新增**，然後加入下列程式碼`<h2>`中新項目*Welcome.cshtml*檔案。 您會建立迴圈，依需求多次使用者指出應該顯示"Hello"。 完整*Welcome.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

執行應用程式，並瀏覽至下列 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

現在從 URL 取得及資料自動傳送到控制器。 控制器封裝資料`ViewBag`物件，以及檢視該物件傳遞。 檢視然後會顯示為 HTML 資料給使用者。

![](adding-a-view/_static/image14.png)

這是一種代表模型的 "M"，但不是資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
