---
title: 將搜尋新增至 ASP.NET Core Razor 頁面
author: rick-anderson
description: 示範如何將搜尋新增至 ASP.NET Core Razor 頁面
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 3900b33f31fef79327d01b0579208355b0bce90c
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410215"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>將搜尋新增至 ASP.NET Core Razor 頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

在下列各節中，會新增依「內容類型」或「名稱」搜尋電影。

將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`：包含使用者在搜尋文字方塊中輸入的文字。 `SearchString` 是以 [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) 屬性裝飾。 `[BindProperty]` 使用與屬性相同的名稱來繫結表單值和查詢字串。 需要 `(SupportsGet = true)` 才能在 GET 要求上進行繫結。
* `Genres`：包含內容類型清單。 `Genres` 可讓使用者從清單中選取內容類型。 `SelectList` 需要 `using Microsoft.AspNetCore.Mvc.Rendering;`
* `MovieGenre`：包含使用者所選取的特定內容類型 (例如「西部片」)。
* 稍後在本教學課程中將會使用 `Genres` 和 `MovieGenre`。

[!INCLUDE[](~/includes/bind-get.md)]

使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

這時候，系統只會「定義」查詢，而尚**未**對資料庫執行查詢。

如果 `SearchString` 屬性不是 Null 或空白，則會修改電影查詢來篩選搜尋字串：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。 定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。 而會延後執行查詢。 這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。 如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。

**注意：**[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。 查詢是否區分大小寫取決於資料庫和定序。 在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。 而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。

巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`https://localhost:5001/Movies?searchString=Ghost`)。 隨即顯示篩選過的電影。

![索引檢視](search/_static/ghost.png)

如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `https://localhost:5001/Movies/Ghost`)。

```cshtml
@page "{searchString?}"
```

上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。  在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

ASP.NET Core 執行階段使用[模型繫結](xref:mvc/models/model-binding)來設定查詢字串 (`?searchString=Ghost`) 中的 `SearchString` 屬性值或路由傳送資料 (`https://localhost:5001/Movies/Ghost`)。 模型繫結是不區分大小寫的。

但是，您不能期望使用者會在搜尋電影時修改 URL。 在此步驟中，會新增用來篩選電影的 UI。 如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。

開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` 標籤會使用下列[標籤協助程式](xref:mvc/views/tag-helpers/intro)：

* [表單標籤協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。 提交表單時，系統會透過查詢字串將篩選字串傳送至 *Pages/Movies/Index* 頁面。
* [輸入標記協助程式](xref:mvc/views/working-with-forms#the-input-tag-helper)

儲存變更並測試篩選條件。

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a>依內容類型搜尋

以下列程式碼更新 `OnGetAsync` 方法：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

內容類型的 `SelectList` 是由投影不同的內容類型來建立。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>將依內容類型搜尋新增至 Razor 頁面

更新 *Index.cshtml*，如下所示：

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。

> [!div class="step-by-step"]
> [上一步：更新頁面](xref:tutorials/razor-pages/da1)
> [下一步：新增欄位](xref:tutorials/razor-pages/new-field)
