---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: "檢查編輯方法與編輯檢視 (C#) |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 137254be69344f31e65e1b4d1318a107ff9d6d47
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>檢查編輯方法與編輯檢視 (C#)
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


在本節中，您將檢驗產生的動作方法和檢視電影控制站。 然後您要加入自訂的搜尋網頁。

執行應用程式，並瀏覽至`Movies`控制器藉由附加*/Movies*至您的瀏覽器的網址列中的 URL。 將滑鼠指標**編輯**連結，查看它連結到的 URL。

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編輯**連結由產生`Html.ActionLink`方法中的*Views\Movies\Index.cshtml*檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

`Html`物件是協助程式上使用屬性公開`WebViewPage`基底類別。 `ActionLink`方法的協助程式簡化了動態產生 HTML，超連結，連結至動作方法，控制站上。 第一個引數`ActionLink`方法是要呈現的連結文字 (例如， `<a>Edit Me</a>`)。 第二個引數是要叫用動作方法的名稱。 最後一個引數是[匿名物件](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)，產生 （在本例中，識別碼，4） 的路由資料。

產生的連結，如上圖所示為`http://localhost:xxxxx/Movies/Edit/4`。 預設路由會採用的 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 會將轉譯`http://localhost:xxxxx/Movies/Edit/4`的要求到`Edit`動作方法的`Movies`控制器會執行參數`ID`等於 4。

您也可以傳遞使用的查詢字串的動作方法參數。 例如，URL`http://localhost:xxxxx/Movies/Edit?ID=4`也會傳遞參數`ID`4 到`Edit`動作方法的`Movies`控制站。

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

開啟`Movies`控制站。 這兩個`Edit`動作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

請注意，第二個 `Edit` 動作方法的前面是 `HttpPost` 屬性。 這個屬性會指定該多載的`Edit`只會針對 POST 要求叫用方法。 您可以套用`HttpGet`第一個屬性編輯方法，但是，因為不需要它是預設值。 (我們將參照動作方法會隱含地指派`HttpGet`屬性做為`HttpGet`方法。)

`HttpGet` `Edit`方法會採用影片 ID 參數在查詢使用 Entity Framework 的電影`Find`方法，並返回編輯檢視中選取的影片。 當 Scaffolding 系統建立 Edit 檢視時，它會檢查 `Movie` 類別，並建立程式碼為類別的每個屬性轉譯 `<label>` 和 `<input>` 元素。 下列範例會顯示產生的編輯檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

請注意如何檢視範本都有`@model MvcMovie.Models.Movie`在檔案最上方的陳述式，這會指定檢視預期型別檢視範本模型`Movie`。

Scaffold 的程式碼會使用數個*helper 方法*來簡化的 HTML 標記。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper 會顯示欄位 （"Title"、"ReleaseDate 」、 「 內容類型"或"Price"） 的名稱。 [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Helper 會顯示為 HTML`<input>`項目。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Helper 會顯示所有與該屬性相關聯的驗證訊息。

執行應用程式，並瀏覽至*/Movies* URL。 按一下 **Edit** 連結。 在瀏覽器中，檢視頁面的原始檔。 HTML 網頁中的看起來像下列的範例。 （為了清楚起見已排除的功能表標記）。

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

`<input>`項目會在 HTML`<form>`項目其`action`屬性設定為張貼到*/電影/編輯*URL。 表單資料都將張貼至伺服器時**編輯**按鈕。

## <a name="processing-the-post-request"></a>處理 POST 要求

下列清單顯示 `HttpPost` 版本的 `Edit` 動作方法。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

ASP.NET framework 模型繫結器會使用的已張貼的表單值，並建立`Movie`物件傳遞為`movie`參數。 `ModelState.IsValid`簽入程式碼會驗證表單中提交的資料可以用於修改`Movie`物件。 如果資料有效，程式碼儲存至電影資料`Movies`集合`MovieDBContext`執行個體。 程式碼再將儲存新的電影資料資料庫呼叫`SaveChanges`方法`MovieDBContext`，其中保存資料庫的變更。 將資料儲存之後, 程式碼將使用者重新導向至`Index`動作方法的`MoviesController`類別，這會導致在電影的清單會顯示更新的影片。

如果張貼的值不是有效的它們會重新顯示在表單中。 `Html.ValidationMessageFor`中的協助程式*Edit.cshtml*檢視範本負責顯示適當的錯誤訊息。

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **請注意，關於地區設定**如果您通常會使用英文以外的地區設定，請參閱[支援 ASP.NET MVC 3 驗證來搭配非英文地區設定。](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>進行更強固的編輯方法

`HttpGet` `Edit` Scaffolding 系統所產生的方法不會檢查傳遞給它的識別碼是否有效。 如果使用者會從 URL 移除識別碼區段 (`http://localhost:xxxxx/Movies/Edit`)，就會顯示下列錯誤：

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

使用者可能也會將傳遞不存在於資料庫中，例如識別碼`http://localhost:xxxxx/Movies/Edit/1234`。 您可以變更兩個`HttpGet``Edit`来解決這項限制動作方法。 首先，變更`ID`參數識別碼不明確地傳遞時，具有預設值是零。 您也可以檢查的`Find`實際上影片物件傳回至檢視範本之前找到電影的方法。 已更新`Edit`方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

如果不找到任何影片，`HttpNotFound`方法呼叫。

所有`HttpGet`方法遵循類似的模式。 他們取得影片物件 (或清單中的物件，大小寫的`Index`)，並將模型傳遞至檢視。 `Create`方法傳遞空影片物件建立檢視。 建立、編輯、刪除或以其他方式修改資料的所有方法都會在方法的 `HttpPost` 多載中執行這個動作。 修改資料中的 HTTP GET 的方法會造成安全性風險，部落格文章項目中所述[ASP.NET MVC 秘訣 #46 – 請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 修改 GET 方法中的資料也違反 HTTP 最佳作法和架構的 REST 模式指定 GET 要求，不應該變更應用程式的狀態。 也就是說，執行取得作業應該是沒有任何副作用是安全的作業。

## <a name="adding-a-search-method-and-search-view"></a>加入搜尋方法和搜尋檢視

本節中您要加入`SearchIndex`動作方法，可讓您搜尋電影依類型或名稱。 這是可以透過*/電影/SearchIndex* URL。 要求將會顯示包含使用者可以填入以便搜尋電影的輸入項目的 HTML 表單。 當使用者提交表單時，動作方法會取得使用者所張貼的搜尋值，並使用值來搜尋資料庫。

## <a name="displaying-the-searchindex-form"></a>顯示 SearchIndex 表單

啟動新增`SearchIndex`至現有的動作方法`MoviesController`類別。 方法會傳回包含 HTML 表單的檢視。 下列程式碼：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

第一行`SearchIndex`方法會建立下列[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查詢，以選取影片：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

此時，定義查詢，但尚未針對資料存放區尚未執行。

如果`searchString`參數包含字串，若要篩選搜尋字串，使用下列程式碼的值修改電影查詢：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

LINQ 查詢不會執行，當它們被定義或修改這些呼叫的方法，例如`Where`或`OrderBy`。 相反地，延後查詢執行，這表示運算式的評估是否延遲到實際反覆查看其實現的值或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)方法呼叫。 在`SearchIndex`範例 SearchIndex 檢視中執行查詢。 如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://msdn.microsoft.com/library/bb738633.aspx)。

現在您可以實作`SearchIndex`會向使用者顯示表單的檢視。 以滑鼠右鍵按一下`SearchIndex`方法，然後按一下**加入檢視**。 在**加入檢視**對話方塊方塊中，指定您要傳遞`Movie`其模型類別檢視範本的物件。 在**Scaffold 範本**清單中，選擇**清單**，然後按一下 **新增**。

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

當您按一下**新增** 按鈕， *Views\Movies\SearchIndex.cshtml*建立檢視範本。 因為您選取**清單**中**Scaffold 範本**清單，Visual Web Developer 中自動產生 (scaffold) 檢視中的某些預設內容。 Scaffolding 建立 HTML 表單。 它檢查`Movie`類別和建立的程式碼來呈現`<label>`類別的每一個屬性的項目。 下列清單顯示建立檢視所產生：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

執行應用程式，並瀏覽至*/電影/SearchIndex*。 將查詢字串 (例如 `?searchString=ghost`) 附加至 URL。 隨即顯示篩選過的電影。

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

如果您變更的簽章`SearchIndex`方法有名稱為的參數`id`、`id`參數將會符合`{id}`預留位置預設路由的設定*Global.asax*檔案。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

修改`SearchIndex`方法可能看起來如下：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

但是，您不能期望使用者在每次想要搜尋電影時修改 URL。 因此，現在您將 UI 來協助它們篩選電影。 如果您已變更的簽章`SearchIndex`方法來測試如何傳遞路由繫結的 ID 參數，將它變更回，讓您`SearchIndex`方法接受字串參數名稱為`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

開啟*Views\Movies\SearchIndex.cshtml*檔案，然後之後`@Html.ActionLink("Create New", "Create")`，加入下列：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

下列範例顯示的某一部分*Views\Movies\SearchIndex.cshtml*檔案以加入篩選標記。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

`Html.BeginForm` Helper 會建立開頭`<form>`標記。 `Html.BeginForm` Helper 會導致表單張貼至本身，當使用者提交表單，即可**篩選** 按鈕。

執行應用程式並再試一次搜尋的電影。

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

沒有任何`HttpPost`多載`SearchIndex`方法。 您不需要它，因為方法不會變更狀態的應用程式，只要篩選資料。

您可以新增下列 `HttpPost SearchIndex` 方法。 在此情況下，會比對動作啟動程式`HttpPost SearchIndex`方法，而`HttpPost SearchIndex`方法會執行下面的影像中所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

不過，即使您新增這個 `HttpPost` 版本的 `SearchIndex` 方法，在如何全部實作此方法方面仍然有其限制。 假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。 請注意，HTTP POST 要求的 URL 是相同的 URL，GET 要求 （localhost:xxxxx/電影/SearchIndex）--在 URL 本身沒有搜尋資訊。 右現在，搜尋字串資訊會傳送到伺服器做為表單欄位值。 這表示您無法擷取書籤，或傳送給朋友在 URL 中搜尋資訊。

解決方案是使用的多載`BeginForm`指定 POST 要求應該加入搜尋資訊的 url，這應該是路由傳送至 HttpGet 版本`SearchIndex`方法。 取代現有無參數`BeginForm`以下列方法：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

現在當您提交搜尋時，URL 中包含的搜尋查詢字串。 即使您有 `HttpPost SearchIndex` 方法，搜尋也會移至 `HttpGet SearchIndex` 動作方法。

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>加入搜尋的影片

如果您加入`HttpPost`版本`SearchIndex`方法，立即將它刪除。

接下來，您要加入的功能，讓使用者搜尋的電影內容類型。 以下列程式碼取代 `SearchIndex` 方法：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

這個版本的`SearchIndex`方法會採用額外的參數，也就是`movieGenre`。 第一個幾行程式碼建立`List`物件來保存從資料庫的電影內容類型。

下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

程式碼會使用`AddRange`方法屬於泛型`List`將不同的內容類型加入至清單的集合。 (不含`Distinct`修飾詞，會加入重複的內容類型 — 例如，喜劇會加入兩次在我們的範例)。 程式碼再儲存清單中的內容類型`ViewBag`物件。

下列程式碼示範如何檢查`movieGenre`參數。 如果不是空的程式碼，進一步限制電影查詢，以限制選取的影片，以指定的內容類型。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>將標記加入至 SearchIndex 檢視，以支援搜尋的影片

新增`Html.DropDownList`協助專家*Views\Movies\SearchIndex.cshtml*檔案之前`TextBox`協助程式。 已完成的標記如下所示：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

執行應用程式，並瀏覽至*/電影/SearchIndex*。 內容類型、 電影名稱，以及這兩個準則，請嘗試搜尋。

本節中您可以檢查的 CRUD 動作方法和架構所產生的檢視。 您可以建立和檢視，讓使用者可以搜尋電影標題和內容類型的搜尋動作方法。 在下一步 區段中，您會看到如何將屬性加入`Movie`模型以及如何將會自動建立測試資料庫的初始設定式。

>[!div class="step-by-step"]
[上一頁](accessing-your-models-data-from-a-controller.md)
[下一頁](adding-a-new-field.md)
