---
title: "Razor 頁面使用 EF Core-排序、 篩選、 分頁-3 個 8"
author: rick-anderson
description: "在本教學課程中，您要加入排序、 篩選和分頁至網頁的 ASP.NET 核心和實體架構的核心功能。"
ms.author: riande
ms.date: 10/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 24649374b71da39d638d943617a219d45f064846
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>排序、 篩選、 分頁和群組的方式-Razor 頁面 (以 8 為 3) 使用的 EF 核心

由[Tom Dykstra](https://github.com/tdykstra)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

在此教學課程、 排序、 篩選、 分組和分頁，請加入功能。

下圖顯示已完成的頁面。 資料行標題會排序資料行可點按的連結。 按一下欄標題時，重複遞增和遞減順序之間切換。

![學生索引頁](sort-filter-page/_static/paging.png)

如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)。

## <a name="add-sorting-to-the-index-page"></a>將排序加入至索引頁面

新增字串*Students/Index.cshtml.cs* `PageModel`包含排序參數：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


更新*Students/Index.cshtml.cs* `OnGetAsync`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

上述程式碼會接收`sortOrder`從 URL 中的查詢字串參數。 （包括查詢字串） 的 URL 由產生[錨定標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder`參數是 「 名稱 」 或 「 日期 」。 `sortOrder`參數 （選擇性） 後面接著"_desc"來指定遞減順序。 預設的排序次序為遞增。

從要求的索引頁面時**學生**連結，沒有查詢字串。 學生姓氏會顯示以遞增順序。 遞增順序依據姓氏是預設值 （中斷的情況），在`switch`陳述式。 當使用者按一下資料行標題連結，適當`sortOrder`查詢字串值中提供值。

`NameSort`和`DateSort`Razor 頁面所用來設定適當的查詢字串值的資料行標題超連結：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

下列程式碼包含 C# [？: 運算子](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

第一行會指定當`sortOrder`是 null 或空白，`NameSort`設為"name_desc。 」 如果`sortOrder`是**不**null 或空白，`NameSort`設為空字串。

`?: operator`就是所謂的三元運算子。

這些兩個陳述式會啟用檢視設定的資料行標題的超連結，如下所示：

| 目前的排序順序 | 最後一個名稱的超連結 | 日期的超連結 |
|:--------------------:|:-------------------:|:--------------:|
| 最後一個名稱升冪 | descending        | ascending      |
| 最後一個遞減的名稱 | ascending           | ascending      |
| 遞增的日期       | ascending           | descending     |
| 日期遞減      | ascending           | ascending      |

方法會使用 LINQ to Entities，來指定排序所依據的資料行。 程式碼會初始化`IQueryable<Student> `switch 陳述式之前，並修改在 switch 陳述式：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 當`IQueryable`是建立或修改任何查詢傳送至資料庫。 不執行查詢，直到`IQueryable`物件轉換成集合。 `IQueryable`會轉換成集合的呼叫方法，例如`ToListAsync`。 因此，`IQueryable`程式碼中不會執行下列陳述式之前的單一查詢的結果：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`無法取得使用大量的資料行的詳細資訊。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>將資料行標題的超連結加入到學生索引檢視

中的程式碼取代*Students/Index.cshtml*，以下列反白顯示程式碼：

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

上述程式碼：

* 加入超連結`LastName`和`EnrollmentDate`資料行標題。
* 使用中的資訊`NameSort`和`DateSort`設定與目前的排序順序值的超連結。

若要確認該排序的運作方式：

* 執行應用程式並選取**學生** 索引標籤。
* 按一下**Last Name**。
* 按一下**註冊日期**。

若要取得更深入了解程式碼：

* 在*Student/Index.cshtml.cs*上, 設定中斷點`switch (sortOrder)`。
* 加入監看式`NameSort`和`DateSort`。
* 在*Student/Index.cshtml*上, 設定中斷點`@Html.DisplayNameFor(model => model.Student[0].LastName)`。

逐步執行偵錯工具。

## <a name="add-a-search-box-to-the-students-index-page"></a>新增搜尋方塊學生索引頁

若要加入篩選學生索引頁：

* 文字方塊和送出按鈕加入至 Razor 頁面。 在文字方塊中提供的搜尋字串的第一個或最後一個名稱。
* 程式碼後置檔案會更新以使用文字方塊的值。

### <a name="add-filtering-functionality-to-the-index-method"></a>索引方法中加入篩選功能

更新*Students/Index.cshtml.cs* `OnGetAsync`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

上述程式碼：

* 新增`searchString`參數`OnGetAsync`方法。 從文字方塊中，加入下一節收到的搜尋字串值。
* 加入 LINQ 陳述式`Where`子句。 `Where`子句選取其名字或姓氏包含搜尋字串的學生。 在 LINQ 陳述式不會執行沒有要搜尋的值。

注意： 上述程式碼會呼叫`Where`方法`IQueryable`物件和篩選器在伺服器上處理。 在某些情況下，可能會呼叫 tha 應用程式`Where`當做擴充方法上的記憶體中集合的方法。 例如，假設`_context.Students`變更從 EF 核心`DbSet`儲存機制的方法，傳回`IEnumerable`集合。 結果通常都是一樣的但在某些情況下可能會不同。

例如，.NET Framework 實作的`Contains`預設會執行區分大小寫的比較。 在 SQL Server，`Contains`區分大小寫取決於 SQL Server 執行個體的定序設定。 SQL Server 的預設值為不區分大小寫。 `ToUpper`無法呼叫來進行測試更明確地不區分大小寫：

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

上述程式碼會確保結果不區分大小寫的程式碼變更為使用`IEnumerable`。 當`Contains`上呼叫`IEnumerable`集合，會使用實作.NET Core。 當`Contains`上呼叫`IQueryable`物件時，會使用資料庫實作。 傳回`IEnumerable`從儲存機制可以有顯著的效能 penality:

1. 從 DB 伺服器傳回的所有資料列。
1. 篩選條件會套用到應用程式中傳回的資料列。

沒有對效能帶來負面影響，用於呼叫`ToUpper`。 `ToUpper`程式碼會在 TSQL SELECT 陳述式的 WHERE 子句中加入函式。 加入函式會防止最佳化工具使用索引。 假設 SQL 安裝為不區分大小寫，就最能夠避免`ToUpper`呼叫時不需要它。

### <a name="add-a-search-box-to-the-student-index-view"></a>學生索引檢視中新增搜尋方塊

在*Views/Student/Index.cshtml*，加入下列反白顯示的程式碼，以建立**搜尋**按鈕和各種的 chrome。

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

上述程式碼會使用`<form>`[標記協助程式](xref:mvc/views/tag-helpers/intro)加入搜尋文字方塊和按鈕。 根據預設，`<form>`標記協助程式送出表單 POST 資料。 使用 post 要求的 HTTP 訊息本文中並不會在 URL 中傳遞參數。 使用 HTTP GET 時，就會在 URL 中表單資料傳遞查詢字串的形式。 使用查詢字串資料傳遞給可讓使用者 URL 加入書籤。 [W3C 指導方針](https://www.w3.org/2001/tag/doc/whenToUseGet.html)建議動作並不會產生更新時，是否應該使用 GET。

測試應用程式：

* 選取**學生**索引標籤並輸入搜尋字串。
* 選取**搜尋**。

請注意 「 URL 」 包含搜尋字串。

```html
http://localhost:5000/Students?SearchString=an
```

如果頁面設為書籤，書籤所包含頁面的 URL 和`SearchString`查詢字串。 `method="get"`中`form`標記是什麼造成要產生的查詢字串。

目前選取的資料行標題排序連結時，篩選值**搜尋**方塊將會遺失。 遺失的篩選值被固定的下一節。

## <a name="add-paging-functionality-to-the-students-index-page"></a>將分頁功能加入至學生索引頁

在本節中，`PaginatedList`類別是用來支援分頁。 `PaginatedList`類別會使用`Skip`和`Take`陳述式來篩選資料，而不會擷取所有資料列之資料表的伺服器上。 下圖顯示 [分頁] 按鈕。

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

在專案資料夾中，建立`PaginatedList.cs`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync`上述程式碼中的方法會採用頁面大小和頁面數目，並套用適當`Skip`和`Take`陳述式，以`IQueryable`。 當`ToListAsync`上呼叫`IQueryable`，它會傳回包含要求的頁面的清單。 屬性`HasPreviousPage`和`HasNextPage`可用來啟用或停用**上一步**和**下一步**分頁按鈕。

`CreateAsync`方法用來建立`PaginatedList<T>`。 無法建立建構函式`PaginatedList<T>`物件建構函式無法執行非同步程式碼。

## <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能加入至索引方法

在*Students/Index.cshtml.cs*，更新的類型`Student`從`IList<Student>`至`PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

更新*Students/Index.cshtml.cs* `OnGetAsync`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

上述程式碼中加入的頁面索引，而目前`sortOrder`，而`currentFilter`方法簽章。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

所有參數都是 null:

* 從網頁呼叫**學生**連結。
* 使用者尚未按一下分頁或排序連結。

按一下分頁連結時，頁面的索引變數包含要顯示的頁面數。

`CurrentSort`提供目前的排序次序 Razor 頁面。 目前的排序次序必須包含在分頁連結，以保留在分頁時的排序次序。

`CurrentFilter`Razor 頁面提供目前的篩選條件字串。 `CurrentFilter`值：

* 必須包含在分頁連結以篩選設定維護期間分頁。
* 必須還原到文字方塊中時的頁面會重新顯示。

如果搜尋字串變更分頁時，網頁會重設為 1。 頁面具有新的篩選器可能會導致不同的資料，以顯示因為重設為 1。 當輸入搜尋值和**送出**選取：

* 變更搜尋字串。
* `searchString`參數不是 null。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync`方法將學生查詢轉換成單一頁面的學生支援分頁的集合型別。 學生的單一頁面會傳遞至 Razor 頁面。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

在兩個問號`PaginatedList.CreateAsync`代表[null 聯合運算子](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator)。 Null 聯合運算子定義預設值為 null 的型別。 運算式`(pageIndex ?? 1)`方法傳回的值`pageIndex`有值。 如果`pageIndex`不會有的值，傳回 1。

## <a name="add-paging-links-to-the-student-razor-page"></a>將 student Razor 頁面中的分頁連結

更新中的標記*Students/Index.cshtml*。 所做的變更會反白顯示：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

資料行頁首連結使用查詢字串傳遞至目前的搜尋字串`OnGetAsync`方法，讓使用者可以排序內篩選結果：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

分頁按鈕會顯示標記協助程式：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

執行應用程式，並瀏覽至學生頁面。

* 若要確定分頁運作，請按一下分頁中的連結不同排序順序。
* 若要驗證分頁正常運作的排序和篩選，輸入搜尋字串並再試一次分頁。

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

若要取得更深入了解程式碼：

* 在*Student/Index.cshtml.cs*上, 設定中斷點`switch (sortOrder)`。
* 加入監看式`NameSort`， `DateSort`， `CurrentSort`，和`Model.Student.PageIndex`。
* 在*Student/Index.cshtml*上, 設定中斷點`@Html.DisplayNameFor(model => model.Student[0].LastName)`。

逐步執行偵錯工具。

## <a name="update-the-about-page-to-show-student-statistics"></a>關於頁面以顯示學生統計資料更新

在此步驟中， *Pages/About.cshtml*已更新為顯示學生數目已達註冊每個註冊日期。 更新使用群組，並包含下列步驟：

* 建立所使用的資料的檢視模型類別**有關**頁面。
* 修改有關 Razor 頁面和程式碼後置檔案。

### <a name="create-the-view-model"></a>建立檢視模型

建立*SchoolViewModels*資料夾中的*模型*資料夾。

在*SchoolViewModels*資料夾中，加入*EnrollmentDateGroup.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-code-behind-page"></a>更新關於程式碼後置頁面

更新*Pages/About.cshtml.cs*以下列程式碼檔案：

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

LINQ 陳述式註冊日期分組的學生實體、 計算每個群組中的實體數目，並將結果儲存在集合中`EnrollmentDateGroup`檢視模型物件。

注意： LINQ `group` EF 核心目前不支援命令。 上述程式碼，在所有學生記錄會都傳回從 SQL Server。 `group` Razor 網頁應用程式中不是 SQL Server 上套用命令。 EF 核心 2.1 將會支援這個 LINQ`group`運算子和群組，就會發生在 SQL Server 上。 請參閱[關聯式： 支援轉譯成 SQL groupby （）](https://github.com/aspnet/EntityFrameworkCore/issues/2341)。 [EF 核心 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap)會隨附.NET Core 2.1。 如需詳細資訊，請參閱[.NET Core 藍圖](https://github.com/dotnet/core/blob/master/roadmap.md)。

### <a name="modify-the-about-razor-page"></a>修改有關 Razor 頁面

中的程式碼取代*Views/Home/About.cshtml*以下列程式碼檔案：

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

執行應用程式，並瀏覽至 [關於] 頁面。 針對每個註冊日期的學生人數會顯示在資料表中。

如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)。

![有關頁面](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>其他資源

* [偵錯 ASP.NET Core 2.x 原始檔](https://github.com/aspnet/Docs/issues/4155)

在下一個教學課程中，應用程式會使用移轉更新的資料模型。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/crud)
[下一頁](xref:data/ef-rp/migrations)
