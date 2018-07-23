---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: 新增檢視 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4a4eacfdcb0b53da377e9b6812a7ce50aa94b551
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831702"
---
<a name="adding-a-view-c"></a>新增檢視 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。
> 
> 
> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


在本節中您要修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。

您將建立使用新的檢視範本檔案[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)使用 ASP.NET MVC 3 引進。 以 razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供優雅的方式來建立 HTML 輸出使用 C#。 Razor 字元和時撰寫檢視範本時，所需按鍵的數目降至最低，並可讓程式碼撰寫工作流程迅速、 流暢。

開始使用檢視範本，內含`Index`方法中的`HelloWorldController`類別。 目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。 變更`Index`方法來傳回`View`物件，如下列所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

此程式碼會使用檢視範本來產生瀏覽器的 HTML 回應。 在專案中，新增您可以使用檢視範本`Index`方法。 若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

![](adding-a-view/_static/image1.png)

**加入檢視** 對話方塊隨即出現。 保留預設值的方式，然後按一下 **新增**按鈕：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。 您可以看到它們**方案總管 中**:

![](adding-a-view/_static/image3.png)

下列所示*Index.cshtml*所建立的檔案：

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

將下的一些 HTML`<h2>`標記。 已修改*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。 `Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，其所指定方法應使用檢視範本檔案來呈現至瀏覽器的回應。 因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*檢視中的檔案*\Views\HelloWorld*資料夾。 下圖顯示字串硬式編碼在檢視中。

![](adding-a-view/_static/image6.png)

看起來很好。 不過，請注意，瀏覽器的標題列會顯示"Index"，並大的標題在頁面上顯示 「 我 MVC 應用程式。 」 讓我們將這些變更。

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

首先，您會想要變更在頁面頂端的 [我的 MVC 應用程式] 標題。 該文字會每個頁面。 它實際上被實作只要在一處，在專案中，即使它出現在應用程式中的每個頁面上。 移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.cshtml*檔案。 這個檔案稱為*版面配置頁*且共用 「 殼層 」 的所有其他頁面使用。

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

版面配置範本可讓您在一個位置指定的 HTML 容器配置，您的網站，然後將它套用到您的網站中的多個頁面。 請注意`@RenderBody()`檔案底部附近的行。 `RenderBody` 是的預留位置，其中您所建立的所有檢視特定頁面都出現，「 包裝的 」 版面配置頁。 變更從 「 我 MVC 應用程式 」 到 「 MVC 電影應用程式 」 的版面配置範本中的標題。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

執行應用程式，並注意現在會顯示 「 MVC 電影應用程式 」。 按一下 **關於**連結，您會看到如何該頁面會顯示 「 MVC 電影應用程式，「 太。 我們能夠在版面配置範本一次進行變更，並已在站台上的所有頁面都反映新的標題。

![](adding-a-view/_static/image9.png)

完整 *\_Layout.cshtml*檔案如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

現在，讓我們變更 （檢視） 中的 [索引] 頁面的標題。

開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。 有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。 您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

若要表示顯示，上述程式碼的 HTML 標題`Title`的屬性`ViewBag`物件 (這是在*Index.cshtml*檢視範本)。 如果您回頭看的版面配置範本的原始程式碼中，您會注意到的範本會使用此值`<title>`一部分的項目`<head>`HTML 一節。 使用這個方法，可以檢視範本和版面配置檔之間輕鬆地傳遞其他參數。

執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。

另外也請注意如何在內容*Index.cshtml*檢視範本與合併 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。

![](adding-a-view/_static/image10.png)

然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!" 訊息) 是硬式編碼的資料。 MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。 不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。 控制器類別會叫用，以回應傳入的 URL 要求。 控制器類別是您撰寫處理傳入的參數，會從資料庫擷取資料和最終決定哪一種回應傳送回瀏覽器的程式碼的位置。 然後從控制器來產生並格式化瀏覽器的 HTML 回應使用檢視範本。

控制器是負責提供任何資料或物件所需為了讓檢視範本來呈現至瀏覽器的回應。 檢視範本應該永遠不會執行商務邏輯，或直接與資料庫互動。 相反地，它應該只使用由控制器提供給它的資料。 維護此 「 關注點分離 」，可協助保護您的程式碼，簡潔且更容易維護。

目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出到瀏覽器直接的值。 與其讓控制器呈現這個回應，做為字串，讓我們變更控制器以改為使用檢視範本。 檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。 您可以透過讓控制器將檢視範本需要的動態資料放置`ViewBag`檢視範本之後可以存取的物件。

返回*HelloWorldController.cs*檔案，並變更`Welcome`方法來加入`Message`並`NumTimes`值`ViewBag`物件。 `ViewBag` 是動態物件，這表示您可以放置任何內容給它;`ViewBag`物件有定義的屬性，直到您將其內的項目。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

現在`ViewBag`物件包含將會自動傳遞至檢視的資料。

接下來，您需要一個歡迎視圖模板！ 在 **偵錯**功能表上，選取**建置 MvcMovie**藉此確定編譯專案。

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。 以下是什麼**加入檢視**對話方塊看起來像：

![](adding-a-view/_static/image13.png)

按一下 **新增**，然後新增下列程式碼底下`<h2>`在新的項目*Welcome.cshtml*檔案。 您將建立多次使用者指出應該顯示"Hello"迴圈。 完整*Welcome.cshtml*檔案如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

執行應用程式，並瀏覽至下列 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

現在資料已從 URL 取得，會自動傳遞到控制器。 控制器將資料封裝成`ViewBag`物件並檢視該物件傳遞。 檢視則會顯示為 HTML 資料給使用者。

![](adding-a-view/_static/image14.png)

這是一種代表模型的 "M"，但不是資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)
