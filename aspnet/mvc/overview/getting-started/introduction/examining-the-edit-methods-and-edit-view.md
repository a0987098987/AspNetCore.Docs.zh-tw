---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "檢查編輯方法與編輯檢視 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>檢查編輯方法與編輯檢視
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本節中，您將會檢查所產生`Edit`動作方法和檢視電影控制站。 但會先進行看起來更好的發行日期簡短轉接。 開啟*Models\Movie.cs*檔案，然後加入反白顯示的行，如下所示：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

您也可以將日期文化特性特定像這樣：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

接下來的教學課程中將涵蓋 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。 [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性指定的資料類型，在此情況下它是日期，因此不會顯示儲存在欄位中的時間資訊。 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性所需的正確轉譯日期格式 Chrome 瀏覽器中的 bug。

執行應用程式，並瀏覽至`Movies`控制站。 將滑鼠指標**編輯**連結，查看它連結到的 URL。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編輯**連結由產生`Html.ActionLink`方法中的*Views\Movies\Index.cshtml*檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`物件是協助程式上使用屬性公開[System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基底類別。 `ActionLink`方法的協助程式簡化了動態產生 HTML，超連結，連結至動作方法，控制站上。 第一個引數`ActionLink`方法是要呈現的連結文字 (例如， `<a>Edit Me</a>`)。 第二個引數是要叫用的動作方法的名稱 (在此情況下，`Edit`動作)。 最後一個引數是[匿名物件](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)，產生 （在本例中，識別碼，4） 的路由資料。

產生的連結，如上圖所示為`http://localhost:1234/Movies/Edit/4`。 預設路由 (在中建立*應用程式\_Start\RouteConfig.cs*) 採用的 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 會將轉譯`http://localhost:1234/Movies/Edit/4`的要求到`Edit`動作方法的`Movies`控制器會執行參數`ID`等於 4。 檢查下列程式碼從*應用程式\_Start\RouteConfig.cs*檔案。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法用來將 HTTP 要求路由至正確的控制器與動作方法並提供選擇性的 ID 參數。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法也會使用[HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx)例如`ActionLink`來產生 Url 指定的控制器、 動作方法和任何路由資料。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

您也可以傳遞使用的查詢字串的動作方法參數。 例如，URL`http://localhost:1234/Movies/Edit?ID=3`也會傳遞參數`ID`為 3`Edit`動作方法的`Movies`控制站。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開啟`Movies`控制站。 這兩個`Edit`動作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

請注意，第二個 `Edit` 動作方法的前面是 `HttpPost` 屬性。 這個屬性指定的多載`Edit`只會針對 POST 要求叫用方法。 您可以套用`HttpGet`第一個屬性編輯方法，但是，因為不需要它是預設值。 (我們將參照動作方法會隱含地指派`HttpGet`屬性做為`HttpGet`方法。)[繫結](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)屬性是防止駭客過度張貼您的模型資料的另一個重要的安全性機制。 您應該只包含您想要變更繫結屬性中的屬性。 您可以閱讀 overposting 和中的繫結屬性我[overposting 安全性注意事項](https://go.microsoft.com/fwlink/?LinkId=317598)。 在此教學課程中使用簡單模型中，我們將繫結模型中的所有資料。 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)屬性用於防範偽造要求，並使用成對向上`@Html.AntiForgeryToken()`在編輯檢視檔案 (*Views\Movies\Edit.cshtml*)，如下所示的一部分：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` 產生隱藏的表單防偽語彙基元必須完全符合`Edit`方法`Movies`控制站。 閱讀更多關於跨網站要求偽造 （也稱為 XSRF 或 CSRF） 在我的教學課程[MVC 中 XSRF/CSRF 防護](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)。

`HttpGet` `Edit`方法會採用影片 ID 參數在查詢使用 Entity Framework 的電影`Find`方法，並返回編輯檢視中選取的影片。 如果找不到電影， [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)傳回。 當 Scaffolding 系統建立 Edit 檢視時，它會檢查 `Movie` 類別，並建立程式碼為類別的每個屬性轉譯 `<label>` 和 `<input>` 元素。 下列範例顯示 visual studio scaffolding 系統所產生的編輯檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

請注意如何檢視範本都有`@model MvcMovie.Models.Movie`在檔案最上方的陳述式，這會指定檢視預期型別檢視範本模型`Movie`。

Scaffold 的程式碼會使用數個*helper 方法*來簡化的 HTML 標記。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper 會顯示欄位的名稱 (&quot;標題&quot;， &quot;ReleaseDate&quot;，&quot;類型&quot;，或&quot;價格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Helper 可呈現 HTML`<input>`項目。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Helper 會顯示所有與該屬性相關聯的驗證訊息。

執行應用程式，並瀏覽至*/Movies* URL。 按一下 **Edit** 連結。 在瀏覽器中，檢視頁面的原始檔。 如下所示的 HTML 表單項目。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>`項目會在 HTML`<form>`項目其`action`屬性設定為張貼到*/電影/編輯*URL。 表單資料都將張貼至伺服器時**儲存**按鈕。 第二行中顯示的隱藏[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)所產生的語彙基元`@Html.AntiForgeryToken()`呼叫。

## <a name="processing-the-post-request"></a>處理 POST 要求

下列清單顯示 `HttpPost` 版本的 `Edit` 動作方法。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)屬性驗證[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)所產生的語彙基元`@Html.AntiForgeryToken()`檢視中呼叫。

[ASP.NET MVC 模型繫結器](https://msdn.microsoft.com/library/dd410405.aspx)採用的已張貼的表單值，並建立`Movie`物件傳遞為`movie`參數。 `ModelState.IsValid` 方法會驗證表單中提交的資料可用於修改 (編輯或更新) `Movie` 物件。 如果是有效的資料，將電影資料會儲存至`Movies`集合`db(MovieDBContext`執行個體)。 將新的電影資料會儲存到資料庫，藉由呼叫`SaveChanges`方法`MovieDBContext`。 儲存資料之後，程式碼將使用者重新導向至 `MoviesController` 類別的 `Index` 動作方法，此方法會顯示電影集合，包括剛剛所進行的變更。

只要用戶端驗證決定欄位的值不是有效的則會顯示錯誤訊息。 如果您停用 JavaScript，就不需要用戶端驗證，但是伺服器會偵測已張貼的值不是有效的並會重新顯示表單值，並顯示錯誤訊息。 稍後在本教學課程中，我們會檢查驗證中更多詳細資料。

`Html.ValidationMessageFor`中的協助程式*Edit.cshtml*檢視範本負責顯示適當的錯誤訊息。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

所有`HttpGet`方法遵循類似的模式。 他們取得影片物件 (或清單中的物件，大小寫的`Index`)，並將模型傳遞至檢視。 `Create`方法傳遞空影片物件建立檢視。 建立、編輯、刪除或以其他方式修改資料的所有方法都會在方法的 `HttpPost` 多載中執行這個動作。 修改資料中的 HTTP GET 的方法會造成安全性風險，部落格文章項目中所述[ASP.NET MVC 秘訣 #46 – 請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 修改 GET 方法中的資料也違反 HTTP 最佳作法和架構[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)模式，以指定 GET 要求，不應該變更應用程式的狀態。 也就是說，執行 GET 作業應該是安全的作業，沒有任何副作用，而且不會修改您的保存資料。

如果您使用英文 （美國） 的電腦，您可以略過本節並移至下一個教學課程。 您可以下載此教學課程的 Globalize 新版[這裡](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)。 如需國際化絕佳兩部分教學課程，請參閱[Nadeem 的 ASP.NET MVC 5 國際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)。


> [!NOTE]
> 若要支援 jQuery 驗證非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點和非英文 （美國） 的日期格式，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用 JavaScript `Globalize.parseFloat`。 您可以從 NuGet 來取得 jQuery 非英文驗證。 （請勿安裝 Globalize 如果您使用英文地區設定。）


1. 從**工具**功能表上，按一下**NuGetLibrary 封裝管理員**，然後按一下 **管理方案的 NuGet 套件**。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. 在左窗格中，選取 **瀏覽*。 * * * （請參閱下面的影像）。
3. 在輸入方塊中，輸入 * Globalize * *。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) 選擇`jQuery.Validation.Globalize`，選擇`MvcMovie`按一下**安裝**。 *Scripts\jquery.globalize\globalize.js*檔案會加入至您的專案。 *Scripts\jquery.globalize\cultures\*資料夾將包含多個文化特性的 JavaScript 檔案。 請注意，可能需要五分鐘才能安裝此套件。

 下列程式碼顯示 Views\Movies\Edit.cshtml 檔案的修改： 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

若要避免重複此程式碼中的每個編輯檢視，您可以將其移動至版面配置檔案。 若要最佳化的指令碼下載，請參閱我教學課程[組合和縮製](../../performance/bundling-and-minification.md)。

如需詳細資訊，請參閱[ASP.NET MVC 3 國際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)和[ASP.NET MVC 3 國際化-第 2 部分 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx)。

為暫時修正，如果您無法取得您的地區設定中使用的驗證，您可以強制電腦使用英文 （美國），或您可以使用停用 JavaScript 瀏覽器。 若要強制電腦使用英文 （美國），您可以將全球化項目加入專案根*web.config*檔案。 下列程式碼會顯示與設定為 英文 （美國） 文化特性的全球化項目。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> 在下一個教學課程中，我們將實作搜尋功能。

>[!div class="step-by-step"]
[上一頁](accessing-your-models-data-from-a-controller.md)
[下一頁](adding-search.md)
