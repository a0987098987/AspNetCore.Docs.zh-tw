---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: 檢查編輯方法與編輯檢視 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a166f6c4450c72adc23f7d36113ceba7e04f1929
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820885"
---
<a name="examining-the-edit-methods-and-edit-view"></a>檢查編輯方法與編輯檢視
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本節中，您將會檢查所產生`Edit`動作方法和檢視電影控制器。 但首先需要簡短的謎進行看起來更漂亮的發行日期。 開啟*Models\Movie.cs*檔案，然後新增反白顯示的線條，如下所示：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

您也可以進行日期文化特性特定像這樣：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

接下來的教學課程中將涵蓋 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。 [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性指定的資料類型，在此情況下它是一個日期，因此不會顯示儲存在欄位中的時間資訊。 [Displayformat 無法](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性需要在 Chrome 瀏覽器中呈現的日期格式不正確的錯誤。

執行應用程式，並瀏覽至`Movies`控制站。 將滑鼠指標停留**編輯**連結，以查看它所連結的 URL。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編輯**所產生連結`Html.ActionLink`中的方法*Views\Movies\Index.cshtml*檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`物件是使用屬性上公開的協助程式[System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基底類別。 `ActionLink`方法的協助程式輕鬆地以動態方式產生連結至動作方法，控制站上的 HTML 超連結。 第一個引數`ActionLink`方法會呈現的連結文字 (例如`<a>Edit Me</a>`)。 第二個引數是要叫用的動作方法的名稱 (在此情況下，`Edit`動作)。 最後一個引數是[匿名物件](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)產生路由資料 （在此案例中，識別碼為 4）。

產生上圖所示的連結是`http://localhost:1234/Movies/Edit/4`。 預設路由 (在中建立*應用程式\_Start\RouteConfig.cs*) 會在 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 會將轉譯`http://localhost:1234/Movies/Edit/4`要求`Edit`動作方法的`Movies`控制站與參數`ID`等於 4。 檢查下列程式碼*應用程式\_Start\RouteConfig.cs*檔案。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法用來將 HTTP 要求路由至正確的控制器和動作方法，並提供選擇性的 ID 參數。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法也會使用[HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx)例如`ActionLink`來產生指定的控制器、 動作方法和任何路由資料的 Url。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

您也可以傳遞使用查詢字串的動作方法參數。 例如，URL`http://localhost:1234/Movies/Edit?ID=3`也會將參數傳遞`ID`為 3，`Edit`動作方法的`Movies`控制站。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開啟`Movies`控制站。 這兩個`Edit`動作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

請注意，第二個 `Edit` 動作方法的前面是 `HttpPost` 屬性。 這個屬性指定的多載`Edit`只會針對 POST 要求叫用方法。 您可以套用`HttpGet`屬性的第一個編輯方法，但並不需要因為它是預設值。 (我們會以隱含方式指派的動作方法`HttpGet`屬性為`HttpGet`方法。)[繫結](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)屬性是可防止駭客過度發佈到您的模型資料的另一個重要的安全性機制。 您應該只包含您想要變更繫結屬性中的屬性。 您可以閱讀大量指派和繫結屬性中的我[（overposting) 的安全性注意事項](https://go.microsoft.com/fwlink/?LinkId=317598)。 在本教學課程使用簡單模型中，我們將繫結模型中的所有資料。 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)屬性用來防範要求偽造，並使用成對`@Html.AntiForgeryToken()`編輯檢視檔案中 (*Views\Movies\Edit.cshtml*)，如下所示的一部分：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` 會產生隱藏的表單的防偽語彙基元必須完全符合`Edit`方法的`Movies`控制站。 您可以深入了解跨站台要求偽造 （也稱為 XSRF 或 CSRF） 在我的教學課程[MVC 中的 XSRF/CSRF 防護](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)。

`HttpGet` `Edit`方法會接受影片識別碼參數、 查詢使用 Entity Framework 電影`Find`方法，並將選取的電影傳回 Edit 檢視。 如果找不到電影， [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)會傳回。 當 Scaffolding 系統建立 Edit 檢視時，它會檢查 `Movie` 類別，並建立程式碼為類別的每個屬性轉譯 `<label>` 和 `<input>` 元素。 下列範例示範的 visual studio scaffolding 系統所產生的 Edit 檢視：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

請注意檢視範本的都有`@model MvcMovie.Models.Movie`在檔案頂端的陳述式，這會指定檢視預期檢視範本類型模型`Movie`。

Scaffold 程式碼使用一些*helper 方法*來簡化 HTML 標記。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)協助程式會顯示欄位的名稱 (&quot;標題&quot;， &quot;ReleaseDate&quot;， &quot;Genre&quot;，或&quot;價格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)協助程式都可呈現 HTML`<input>`項目。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)協助程式會顯示與該屬性相關聯的任何驗證訊息。

執行應用程式，並瀏覽至 */Movies* URL。 按一下 **Edit** 連結。 在瀏覽器中，檢視頁面的原始檔。 如下所示的 HTML 表單項目。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>`項目會在 HTML`<form>`項目其`action`屬性設定為發佈到 */Movies/編輯*URL。 將表單資料將會張貼至伺服器時**儲存**按一下按鈕時。 第二行中顯示的隱藏[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)所產生的語彙基元`@Html.AntiForgeryToken()`呼叫。

## <a name="processing-the-post-request"></a>處理 POST 要求

下列清單顯示 `HttpPost` 版本的 `Edit` 動作方法。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)屬性會驗證[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)所產生的語彙基元`@Html.AntiForgeryToken()`檢視中呼叫。

[ASP.NET MVC 模型繫結](https://msdn.microsoft.com/library/dd410405.aspx)採用表單的值，並建立`Movie`物件傳遞為`movie`參數。 `ModelState.IsValid` 方法會驗證表單中提交的資料可用於修改 (編輯或更新) `Movie` 物件。 如果資料是有效的電影資料會儲存至`Movies`的集合`db(MovieDBContext`執行個體)。 新的電影資料會儲存到資料庫，藉由呼叫`SaveChanges`方法的`MovieDBContext`。 儲存資料之後，程式碼將使用者重新導向至 `MoviesController` 類別的 `Index` 動作方法，此方法會顯示電影集合，包括剛剛所進行的變更。

只要用戶端驗證會決定欄位的值不是有效，則會顯示一則錯誤訊息。 如果您停用 JavaScript，就不需要用戶端驗證，但是伺服器會偵測到已張貼的值不是有效的並會重新顯示表單值，並顯示錯誤訊息。 稍後在本教學課程中，我們會檢視更詳細的驗證。

`Html.ValidationMessageFor`中的協助程式*Edit.cshtml*檢視範本負責顯示適當的錯誤訊息。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

所有`HttpGet`方法遵循類似的模式。 他們會取得電影物件 (或物件的清單，這種案例`Index`)，並將模型傳遞至檢視。 `Create`方法會將空白電影物件傳遞給建立檢視。 建立、編輯、刪除或以其他方式修改資料的所有方法都會在方法的 `HttpPost` 多載中執行這個動作。 修改資料中的 HTTP GET 方法會造成安全性風險，部落格張貼文章中所述[ASP.NET MVC 秘訣 #46 – 不使用刪除連結，因為它們會造成安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 修改的 GET 方法中的資料也會違反 HTTP 最佳做法和架構[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)模式指定 GET 要求不應該變更應用程式的狀態。 也就是說，執行 GET 作業應該是安全的作業，沒有任何副作用，而且不會修改您的保存資料。

如果您使用的英文 （美國） 的電腦，您可以略過本節並移至下一個教學課程。 您可以下載本教學課程的 Globalize 新版[此處](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)。 如需國際化的絕佳兩部分教學課程，請參閱 < [Nadeem 的 ASP.NET MVC 5 國際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)。


> [!NOTE]
> 以 jQuery 驗證支援非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點和非英文日期格式，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用的 JavaScript `Globalize.parseFloat`。 您可以從 NuGet 取得 jQuery non-alpha Non-english 驗證。 （不安裝 Globalize 如果您使用英文的地區設定）。


1. 從**工具**功能表上，按一下**NuGetLibrary Package Manager**，然後按一下**管理方案的 NuGet 套件**。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. 在左窗格中，選取<strong>瀏覽 *。</strong>*（見下圖）。
3. 在輸入方塊中，輸入 * Globalize * *。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) 選擇`jQuery.Validation.Globalize`，選擇`MvcMovie`然後按一下**安裝**。 *Scripts\jquery.globalize\globalize.js*檔案會加入至專案。 *Scripts\jquery.globalize\cultures\*資料夾將包含多個文化特性 JavaScript 檔案。 請注意，可能需要五分鐘的時間安裝這個套件。

   下列程式碼顯示修改 Views\Movies\Edit.cshtml 檔案： 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

若要避免重複此程式碼在每個編輯檢視中的，您可以移到版面配置檔。 若要最佳化指令碼下載，請參閱我的教學課程[統合和縮製](../../performance/bundling-and-minification.md)。

如需詳細資訊，請參閱[ASP.NET MVC 3 國際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)並[ASP.NET MVC 3 國際化-第 2 部分 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx)。

為暫時的修正程式，如果您無法取得您的地區設定中使用的驗證，您可以強制電腦以使用英文 （美國），或您可以在瀏覽器中停用 JavaScript。 若要強制您的電腦，以使用英文 （美國），您可以將 globalization 項目加入專案根目錄*web.config*檔案。 下列程式碼會顯示與設為 英文 （美國） 文化特性的全球化項目。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> 在下一個教學課程中，我們將實作搜尋功能。

> [!div class="step-by-step"]
> [上一頁](accessing-your-models-data-from-a-controller.md)
> [下一頁](adding-search.md)
