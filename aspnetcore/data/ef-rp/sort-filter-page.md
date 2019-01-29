---
title: ASP.NET Core 中的 Razor 頁面與 EF Core：排序、篩選、分頁 - 3/8
author: rick-anderson
description: 在本教學課程中，您將會使用 ASP.NET Core 和 Entity Framework Core 將排序、篩選、分頁功能新增至頁面。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 350243fb94b4798293a5a61b580c3b3b4d8c6d4a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444294"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core：排序、篩選、分頁 - 3/8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

在本教學課程中，將新增排序、篩選、分組和分頁功能。

下圖顯示已完成的頁面。 資料行標題為可按式連結，可用以排序資料行。 重覆按一下資料行標題，可切換遞增和遞減排序次序。

![Students [索引] 頁面](sort-filter-page/_static/paging.png)

若您遇到無法解決的問題，請下載[完整應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。

## <a name="add-sorting-to-the-index-page"></a>將排序新增至索引頁面

新增字串至 *Students/Index.cshtml.cs*`PageModel` 以包含排序參數：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

以下列程式碼更新 *Students/Index.cshtml.cs* `OnGetAsync` ：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

上述程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。 URL (包括查詢字串) 是由[錨點標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)所產生

`sortOrder`參數為 "Name" 或 "Date"。 `sortOrder` 參數後面可以選擇接著 "_desc" 來指定遞減順序。 預設的排序次序為遞增。

從 **Students** 連結要求 [索引] 頁面時，將不會有查詢字串。 學生會以遞增姓氏順序顯示。 在 `switch` 陳述式中，預設會依姓氏遞增排序 (fall-through 大小寫)。 使用者按一下資料行標題連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。

Razor 頁面會以適當的查詢字串值，使用 `NameSort` 和 `DateSort` 來設定資料行標題超連結：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

下列程式碼包含 C# 條件式 [?: 運算子](/dotnet/csharp/language-reference/operators/conditional-operator)：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

第一行指定當 `sortOrder` 為 null 或空白時，將 `NameSort` 設為 "name_desc"。 如果 `sortOrder` **不是** null 或空白，則 `NameSort` 設為空字串。

`?: operator` 也是所謂的三元運算子。

這兩個陳述式讓頁面能夠設定資料行標題超連結，如下所示：

| 目前排序次序 | 姓氏超連結 | 日期超連結 |
|:--------------------:|:-------------------:|:--------------:|
| 姓氏遞增 | descending        | ascending      |
| 姓氏遞減 | ascending           | ascending      |
| 日期遞增       | ascending           | descending     |
| 日期遞減      | ascending           | ascending      |

此方法使用 LINQ to Entities 來指定排序所依據的資料行。 這個程式碼會在 switch 陳述式之前初始化 `IQueryable<Student>`，並在 switch 陳述式中將其修改：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 建立或修改 `IQueryable` 時，不會有任何查詢傳送至資料庫。 直到 `IQueryable` 物件轉換成集合前，將不會執行查詢。 `IQueryable` 會藉由呼叫像是 `ToListAsync` 的方法，轉換成集合。 因此，`IQueryable` 程式碼會成為一個單一查詢且不會執行，直到下列陳述式產生：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` 可取得使用大量可排序資料行數的詳細資訊。

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>將資料行標題超連結新增至 Student 的 [索引] 頁面

用下列醒目標示的程式碼，取代 *Students/Index.cshtml* 中的程式碼：

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

上述程式碼：

* 將超連結新增至 `LastName` 和 `EnrollmentDate` 資料行標題。
* 使用 `NameSort` 和 `DateSort` 的資訊，以目前排序次序值來設定超連結。

若要確認該排序運作正常：

* 執行應用程式並選取 [Students] 索引標籤。
* 按一下 [姓氏]。
* 按一下 [註冊日期]。

若要更深入了解這個程式碼：

* 在 *Students/Index.cshtml.cs* 的 `switch (sortOrder)` 上，設定中斷點。
* 為 `NameSort` 和 `DateSort` 新增監看式。
* 在 *Students/Index.cshtml* 的 `@Html.DisplayNameFor(model => model.Student[0].LastName)` 上，設定中斷點。

逐步執行偵錯工具。

## <a name="add-a-search-box-to-the-students-index-page"></a>將搜尋方塊新增至 Students [索引] 頁面

將篩選新增至 Students [索引] 頁面：

* 文字方塊和提交按鈕會新增至 Razor 頁面。 文字方塊提供名字或姓氏的搜尋字串。
* 頁面模型會更新為使用文字方塊的值。

### <a name="add-filtering-functionality-to-the-index-method"></a>將篩選功能新增至 Index 方法

以下列程式碼更新 *Students/Index.cshtml.cs* `OnGetAsync` ：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

上述程式碼：

* 將 `searchString` 參數新增至 `OnGetAsync` 方法。 從文字方塊中接收搜尋字串值文字方塊會在下一節新增。
* 將 `Where` 子句新增至 LINQ 陳述式。 `Where` 子句只會選取名字或姓氏包含搜尋字串的學生。 有可以搜尋的值，LINQ 陳述式才會執行。

注意:上面的程式碼會呼叫 `IQueryable` 物件上的 `Where` 方法，而且會在伺服器上處理篩選。 在某些情況下，應用程式可能會呼叫 `Where` 方法在記憶體內部集合上作為擴充方法。 例如，假設 `_context.Students` 從 EF Core `DbSet` 變為傳回 `IEnumerable` 集合的儲存機制方法。 結果通常都是一樣的，但在某些情況下可能會不同。

例如，.NET Framework 的 `Contains` 實作，預設會執行區分大小寫的比較。 在 SQL Server，`Contains` 區分大小寫取決於 SQL Server 執行個體的定序設定。 SQL Server 預設為不區分大小寫。 可以使用 `ToUpper` 使測試明確不區分大小寫：

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

如果程式碼變更為使用 `IEnumerable`，則上述程式碼會確保結果不區分大小寫。 在 `IEnumerable` 集合上呼叫 `Contains` 時，會使用 .NET Core 實作。 在 `IQueryable` 物件上呼叫 `Contains` 時，會使用資料庫實作。 從儲存機制中傳回 `IEnumerable` 時，效能可能會顯著損失：

1. 所有列都會從資料庫伺服器傳回。
1. 在應用程式中，傳回的所有資料列都會套用篩選。

呼叫 `ToUpper` 會使效能降低。 `ToUpper` 程式碼會將一個函式新增至 TSQL SELECT 陳述式的 WHERE 子句中。 新增的函式會防止最佳化工具使用索引。 假如 SQL 已安裝為不區分大小寫，除非有需要，否則應盡量避免呼叫 `ToUpper`。

### <a name="add-a-search-box-to-the-student-index-page"></a>將搜尋方塊新增至學生的 [索引] 頁面

在 *Pages/Students/Index.cshtml* 中新增下列醒目標示的程式碼，以建立 **Search** 按鈕和各式各樣的色彩。

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

上述程式碼會使用 `<form>` [標籤協助程式](xref:mvc/views/tag-helpers/intro) 新增搜尋文字方塊和按鈕。 `<form>` 標籤協助程式預設為使用 POST 提交表單資料。 在 POST 中，參數會在 HTTP 訊息本文中傳遞，而不是在 URL 中傳遞。 使用 HTTP GET 時，表單資料會以查詢字串的形式在 URL 中傳遞。 以查詢字串來傳遞資料，可讓使用者為 URL 加上書籤。 [W3C 指導方針](https://www.w3.org/2001/tag/doc/whenToUseGet.html) 建議，只有在動作不會產生更新時才應使用 GET。

測試應用程式：

* 選取 [Students] 索引標籤並輸入搜尋字串。
* 選取 [搜尋]。

請注意 URL 中包含了搜尋字串。

```html
http://localhost:5000/Students?SearchString=an
```

如果頁面已加上書籤，那麼書籤會包含該頁面的 URL 和 `SearchString` 查詢字串。 `form` 中的 `method="get"` 導致查詢字串的產生。

目前，選取資料行標題排序連結時，[搜尋] 方塊中的篩選值將會遺失。 遺失的篩選值會在下一節修正。

## <a name="add-paging-functionality-to-the-students-index-page"></a>將分頁功能新增至 Students [索引] 頁面

在本節中，`PaginatedList` 類別用來支援分頁。 `PaginatedList` 類別會使用 `Skip` 和 `Take` 陳述式來篩選伺服器上的資料，而不會擷取資料表中的所有資料列。 下圖顯示分頁按鈕。

![有分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

在專案資料夾中，以下列程式碼建立 `PaginatedList.cs`：

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

上述程式碼中的 `CreateAsync` 方法會採用頁面大小和頁面數，並會將適當的 `Skip` 和 `Take` 陳述式套用至 `IQueryable`。 在 `IQueryable` 上呼叫 `ToListAsync` 時，會傳回僅包含所要求頁面的清單。 `HasPreviousPage` 和 `HasNextPage` 屬性可用來啟用或停用 **Previous** 和 **Next** 分頁按鈕。

`CreateAsync` 方法為建立 `PaginatedList<T>` 之用。 建構函式無法建立 `PaginatedList<T>` 物件，建構函式也無法執行非同步程式碼。

## <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能新增至 Index 方法

在 *Students/Index.cshtml.cs* 中，將 `Student` 的類型從 `IList<Student>` 更新至 `PaginatedList<Student>`：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

以下列程式碼更新 *Students/Index.cshtml.cs* `OnGetAsync` ：

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

上述程式碼將頁面索引、目前的 `sortOrder`、`currentFilter` 新增至方法簽章。

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

遇下列情況時，所有的參數都會成為 null：

* 從 **Students** 連結呼叫頁面。
* 使用者沒有按一下分頁或排序連結。

按一下分頁連結時，頁面索引變數即包含要顯示的頁面數。

`CurrentSort` 提供 Razor 頁面目前的排序次序。 目前的排序次序必須包含在分頁連結中，以保留分頁時的排序次序。

`CurrentFilter` 提供 Razor 頁面目前的篩選字串。 `CurrentFilter` 值：

* 必須包含在分頁連結中，以保留分頁時的篩選設定。
* 頁面重新顯示時，必須還原到文字方塊中。

如果搜尋字串在分頁時變更，頁面會重設為 1。 頁面必須重設為 1 是因為新的篩選可能會導致顯示不同的資料。 輸入搜尋值和選取 **Submit** 時：

* 搜尋字串變更。
* `searchString` 參數不是 null。

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` 方法會以支援分頁的集合類型，將學生查詢轉換成學生單一頁面。 該學生單一頁面會傳遞至 Razor 頁面。

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

在 `PaginatedList.CreateAsync` 中的兩個問號代表 [null 聯合運算子](/dotnet/csharp/language-reference/operators/null-conditional-operator)。 Null 聯合運算子會針對可為 Null 的型別定義一個預設值。 運算式 `(pageIndex ?? 1)` 表示，如果 `pageIndex` 有一個值就將該值傳回。 如果 `pageIndex` 沒有值，則傳回 1。

## <a name="add-paging-links-to-the-student-razor-page"></a>將分頁連結新增至學生 Razor 頁面

更新 *Students/Index.cshtml* 中的標記。 所做的變更已醒目標示：

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

資料行頁首連結會使用查詢字串，將目前的搜尋字串傳遞至 `OnGetAsync` 方法，讓使用者可以在篩選結果內排序：

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

分頁按鈕會由標籤協助程式來顯示：

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

執行應用程式並巡覽至學生頁面。

* 若要確定分頁運作正常，請以不同排序次序按一下分頁連結。
* 若要驗證分頁的排序和篩選能正確運作，請輸入搜尋字串並嘗試分頁。

![有分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

若要更深入了解這個程式碼：

* 在 *Students/Index.cshtml.cs* 的 `switch (sortOrder)` 上，設定中斷點。
* 為 `NameSort`、`DateSort`、`CurrentSort`、`Model.Student.PageIndex` 新增監看式。
* 在 *Students/Index.cshtml* 的 `@Html.DisplayNameFor(model => model.Student[0].LastName)` 上，設定中斷點。

逐步執行偵錯工具。

## <a name="update-the-about-page-to-show-student-statistics"></a>更新 About 頁面以顯示學生統計資料

在此步驟中，*Pages/About.cshtml* 會更新為顯示在每一個註冊日期中，共有多少學生註冊。 此更新會使用群組，並包含下列步驟：

* 為 **About** 頁面所使用的資料，建立檢視模型。
* 更新 About 頁面以使用檢視模型。

### <a name="create-the-view-model"></a>建立檢視模型

在 *Models* 資料夾中建立 *SchoolViewModels* 資料夾。

在 *SchoolViewModels* 資料夾中，以下列程式碼新增 *EnrollmentDateGroup.cs*：

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>更新 About 頁面模型

ASP.NET Core 2.2 中的 Web 範本不包括 [關於] 頁面。 若您使用 ASP.NET Core 2.2，請建立 [關於 Razor Page] 頁面。

以下列程式碼更新 *Pages/About.cshtml.cs* 檔案：

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。

### <a name="modify-the-about-razor-page"></a>修改 About Razor 頁面

以下列程式碼取代 *Pages/About.cshtml* 檔案中的程式碼：

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

執行應用程式並巡覽至 About 頁面。 每個註冊日期的學生人數將會顯示在資料表中。

如果您遇到無法解決的問題，請下載[此階段完成的應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)。

![About 頁面](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>其他資源

* [偵錯 ASP.NET Core 2.x 原始檔](https://github.com/aspnet/Docs/issues/4155)

在下一個教學課程中，應用程式將會使用移轉來更新資料模型。

::: moniker-end

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/crud)
> [下一頁](xref:data/ef-rp/migrations)
