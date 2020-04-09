---
title: 將搜尋新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 示範如何將搜尋新增至基本 ASP.NET Core MVC 應用程式
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 89f1fa84783430f160ca0b840bf7ae9699520cb7
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78662866"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>將搜尋新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您會將搜尋功能新增至 `Index` 動作方法，讓您依據「內容類型」** 或「名稱」** 搜尋電影。

使用下列程式碼更新在 *Controllers/MoviesController.cs* 中找到的 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

`Index` 動作方法的第一行會建立 [LINQ](/dotnet/standard/using-linq) 查詢，以選取電影：

```csharp
var movies = from m in _context.Movie
             select m;
```

這時候，系統只會「定義」查詢**，而尚**未**對資料庫執行查詢。

如果 `searchString` 參數包含字串，則會修改電影查詢來篩選搜尋字串的值：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

上述 `s => s.Title.Contains()` 程式碼是 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 在以方法為基礎的 [LINQ](/dotnet/standard/using-linq) 查詢中，Lambda 會用來作為標準查詢運算子方法的引數，例如 [Where](/dotnet/api/system.linq.enumerable.where) 方法或 `Contains` (用於上述程式碼)。 定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會執行查詢。 而會延後執行查詢。  這是指延遲評估運算式，直到實際反覆運算其實現值或呼叫 `ToListAsync` 方法為止。 如需延後查詢執行的詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。

注意：[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在上方顯示的 C# 程式碼中執行。 查詢是否區分大小寫取決於資料庫和定序。 在 SQL Server 上，[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。 而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。

瀏覽至 `/Movies/Index`。 將查詢字串 (例如 `?searchString=Ghost`) 附加至 URL。 隨即顯示篩選過的電影。

![索引檢視](~/tutorials/first-mvc-app/search/_static/ghost.png)

如果您將 `Index` 方法的簽章變更為包含名為 `id` 的參數，則 `id` 參數會比對 *Startup.cs* 中所設定之預設路由的選擇性 `{id}` 預留位置。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

將參數變更為 `id`，並將所有出現的 `searchString` 變更為 `id`。

前一個 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

含有 `id` 參數的已更新 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](~/tutorials/first-mvc-app/search/_static/g2.png)

但是，您不能期望使用者在每次想要搜尋電影時修改 URL。 因此，現在您將新增可協助他們篩選電影的 UI 項目。 如果您已變更 `Index` 方法的簽章來測試如何傳遞路由繫結的 `ID` 參數，請將其變更回採用一個名為 `searchString` 參數：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

開啟 *Views/Movies/Index.cshtml* 檔案，並新增下面強調顯示的 `<form>` 標記：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` 標記使用[表單標記協助程式](xref:mvc/views/working-with-forms)，因此當您提交表單時，篩選條件字串會張貼至電影控制器的 `Index` 動作。 儲存變更，然後測試篩選條件。

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](~/tutorials/first-mvc-app/search/_static/filter.png)

沒有您可能期望的 `Index` 方法的 `[HttpPost]` 多載。 您不需要它，因為方法不會變更應用程式的狀態，而只會篩選資料。

您可以新增下列 `[HttpPost] Index` 方法。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` 參數用來建立 `Index` 方法的多載。 我們稍後將在本教學課程中加以討論。

如果您新增此方法，動作啟動程式會比對 `[HttpPost] Index` 方法，而 `[HttpPost] Index` 方法會如下列影像所示執行。

![應用程式回應為 "From HttpPost Index: filter on ghost" 的瀏覽器視窗](~/tutorials/first-mvc-app/search/_static/fo.png)

不過，即使您新增這個 `[HttpPost]` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。 假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。 請注意，HTTP POST 與 GET 要求的 URL (localhost:{PORT}/Movies/Index) 相同 -- 在 URL 中沒有搜尋資訊。 搜尋字串資訊會以[表單欄位值](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)的形式傳送至伺服器。 您可以使用瀏覽器開發人員工具或絕佳的 [Fiddler 工具](https://www.telerik.com/fiddler)來進行確認。 下圖顯示 Chrome 瀏覽器開發人員工具：

![顯示 searchString 值為 ghost 之要求本文的 Microsoft Edge 開發人員工具的 [網路] 索引標籤](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

您可以在要求本文中看到搜尋參數和 [XSRF](xref:security/anti-request-forgery) 語彙基元。 請注意，如先前的教學課程中所述，[表單標記協助程式](xref:mvc/views/working-with-forms)會產生 [XSRF](xref:security/anti-request-forgery) 防偽語彙基元。 我們不會修改資料，因此不需要驗證控制器方法中的語彙基元。

由於搜尋參數是在要求本文而不是 URL 中，因此您無法擷取該搜尋資訊以加為書籤或與其他人共用。 指定要求應為在 *Views/Movies/Index.cshtml* 檔案中找到的 `HTTP GET`，來修正此問題。

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

現在當您提交搜尋時，URL 中會包含搜尋查詢字串。 即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。

![顯示 URL 中 searchString=ghost 且傳回的電影 Ghostbusters 和 Ghostbusters 2 包含 ghost 一詞的瀏覽器視窗](~/tutorials/first-mvc-app/search/_static/search_get.png)

下列標記顯示 `form` 標記的變更：

```cshtml
<form asp-controller="Movies" asp-action="Index" method="get">
```

## <a name="add-search-by-genre"></a>新增依內容類型搜尋

將下列 `MovieGenreViewModel` 類別新增至 *Models* 資料夾：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

電影內容類型檢視模型將包含：

* 電影清單。
* 包含內容類型清單的 `SelectList`。 這可讓使用者從清單中選取內容類型。
* 包含所選取內容類型的 `MovieGenre`。
* `SearchString`，其中包含使用者在搜尋文字方塊中輸入的文字。

以下列程式碼取代 `MoviesController.cs` 中的 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

下列程式碼是從資料庫中擷取所有內容類型的 `LINQ` 查詢。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

藉由投射不同的內容類型來建立內容類型的 `SelectList` (我們不希望選取清單中有重複的內容類型)。

當使用者搜尋項目時，搜尋的值會保留在搜尋方塊中。

## <a name="add-search-by-genre-to-the-index-view"></a>將依內容類型搜尋新增至 Index 檢視

更新在 *Views/Movies/* 中找到的 `Index.cshtml`，如下所示：

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,19,28,31,34,37,43)]

檢查下列 HTML 協助程式中使用的 Lambda 運算式：

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

在上述程式碼中，`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。 由於 Lambda 運算式是檢查而不是評估，因此您在 `model`、`model.Movies` 或 `model.Movies[0]` 是 `null` 或空白時，不會收到存取違規。 在評估 Lambda 運算式時 (例如，`@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。

依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式：

![顯示 https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2 結果的瀏覽器視窗](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> [前一個](controller-methods-views.md)
> [下一個](new-field.md)
