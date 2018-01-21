---
title: "ASP.NET Core MVC EF 核心-排序、 篩選、 分頁-10-3"
author: tdykstra
description: "在本教學課程中，您要加入排序、 篩選和分頁至網頁的 ASP.NET 核心和實體架構的核心功能。"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 6da2073b18f6fff9738808c84441e59240caefe3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>排序、 篩選、 分頁和群組-EF Core 與 ASP.NET Core MVC 教學課程 (10-3)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您可以實作網頁學生實體的基本 CRUD 作業的一組。 在此教學課程中您要加入排序、 篩選和分頁功能學生索引頁。 此外，還要建立會執行簡單群組頁面。

下圖顯示當您完成頁面的外觀。 資料行標題是使用者可以按一下以依據該資料行排序的連結。 按一下欄標題重複遞增和遞減順序之間切換。

![學生索引頁](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>將資料行排序連結加入至學生索引頁

若要加入排序學生索引頁，您要變更`Index`學生控制器方法並將程式碼加入至學生索引檢視。

### <a name="add-sorting-functionality-to-the-index-method"></a>加入排序功能索引方法

在*StudentsController.cs*，取代`Index`方法取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

此程式碼會接收`sortOrder`從 URL 中的查詢字串參數。 查詢字串值是由 ASP.NET Core MVC 提供，做為參數至動作方法。 參數會是可選擇性地接著底線加上字串"desc"來指定遞減順序為"Name"Date"的字串。 預設的排序次序為遞增。

第一次要求的索引頁面時，沒有任何查詢字串。 學生姓氏，而且是在中斷的情況中所建立的預設值會顯示以遞增順序`switch`陳述式。 當使用者按一下資料行標題超連結，適當`sortOrder`查詢字串中提供值。

這兩個`ViewData`（NameSortParm 和 DateSortParm） 的項目可用的檢視來設定適當的查詢字串值的資料行標題超連結。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

這些是三元陳述式。 第一個指定，如果`sortOrder`參數為 null 或空的 NameSortParm 應該設定為"name_desc 」; 否則它應該設定為空字串。 這些兩個陳述式會啟用檢視設定的資料行標題的超連結，如下所示：

|  目前的排序順序  | 最後一個名稱的超連結 | 日期的超連結 |
|:--------------------:|:-------------------:|:--------------:|
| 最後一個名稱升冪  | descending          | ascending      |
| 最後一個遞減的名稱 | ascending           | ascending      |
| 遞增的日期       | ascending           | descending     |
| 日期遞減      | ascending           | ascending      |

方法會使用 LINQ to Entities，來指定排序所依據的資料行。 程式碼會建立`IQueryable`變數 switch 陳述式前的加以修改在 switch 陳述式，並呼叫`ToListAsync`方法之後`switch`陳述式。 當您建立和修改`IQueryable`變數沒有查詢傳送至資料庫。 查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToListAsync`。 因此，此程式碼會產生單一查詢，將會等到執行`return View`陳述式。

此程式碼可以取得詳細資訊，使用大量的資料行。 [此系列中的最後一個教學課程](advanced.md#dynamic-linq)示範如何撰寫程式碼，可讓您傳遞的名稱`OrderBy`字串變數中的資料行。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>將資料行標題的超連結加入到學生索引檢視

中的程式碼取代*Views/Students/Index.cshtml*，若要將資料行標題的超連結加入下列程式碼。 反白顯示已變更的行。

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

此程式碼使用中的資訊`ViewData`屬性，以設定適當的查詢與超連結的字串值。

執行應用程式中，選取**學生**索引標籤，然後按一下**姓氏**和**註冊日期**以確認該排序的資料行標題的運作方式。

![學生名稱順序的索引頁](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>新增搜尋方塊學生索引頁

若要加入篩選學生索引頁，您會將文字方塊及提交按鈕加入至檢視，並進行中的對應變更`Index`方法。 在文字方塊中，可讓您輸入要搜尋的名字和最後一個名稱欄位中的字串。

### <a name="add-filtering-functionality-to-the-index-method"></a>索引方法中加入篩選功能

在*StudentsController.cs*，取代`Index`（所做的變更會反白顯示） 為下列程式碼的方法。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

您已新增`searchString`參數`Index`方法。 從文字方塊中，您會加入至索引檢視收到的搜尋字串值。 您已也加入 LINQ 陳述式 where 子句，會選取其名字或姓氏包含搜尋字串的學生。 陳述式加入 where 子句在沒有要搜尋的值時，才會執行。

> [!NOTE]
> 這裡您將會呼叫`Where`方法`IQueryable`物件和篩選器會在伺服器上處理。 在某些情況下您可能會呼叫`Where`當做擴充方法上的記憶體中集合的方法。 (例如，假設您變更參考`_context.Students`因此，而是的 EF`DbSet`它所參考的儲存機制的方法會傳回`IEnumerable`集合。)結果通常都是一樣的但在某些情況下可能會不同。
>
>例如，.NET Framework 實作的`Contains`方法會根據預設，執行區分大小寫的比較，但 SQL Server 中這取決於 SQL Server 執行個體的定序設定。 該設定就會預設為不區分大小寫。 您可以呼叫`ToUpper`來進行測試更明確地不區分大小寫的方法：*其中 (s = > s.LastName.ToUpper()。Contains(searchString.ToUpper())*。 會確保結果保持相同如果您變更程式碼，稍後要使用的儲存機制傳回`IEnumerable`集合，而不是`IQueryable`物件。 (當您呼叫`Contains`方法`IEnumerable`集合，取得.NET Framework 實作，則當您呼叫它在`IQueryable`物件，則會收到資料庫提供者實作。)不過，沒有針對此解決方案對效能帶來負面影響。 `ToUpper`程式碼會置於 TSQL SELECT 陳述式的 WHERE 子句的函式。 會防止最佳化工具使用索引。 假設 SQL 大部分安裝為不區分大小寫，就最能夠避免`ToUpper`程式碼，直到您將移轉至區分大小寫的資料存放區。

### <a name="add-a-search-box-to-the-student-index-view"></a>學生索引檢視中新增搜尋方塊

在*Views/Student/Index.cshtml*，開頭才能建立 標題 文字方塊中，表格標記之前加入反白顯示程式碼和**搜尋** 按鈕。

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

此程式碼使用`<form>`[標記協助程式](xref:mvc/views/tag-helpers/intro)加入搜尋文字方塊和按鈕。 根據預設，`<form>`標記協助程式送出表單資料的文章時，這表示，參數傳遞的 HTTP 訊息本文，而不是在 URL 查詢字串的形式。 當您指定 HTTP GET 時，表單資料會在 URL 中當做傳遞查詢字串，可讓使用者 URL 加入書籤。 此動作不會導致更新時，就會收到 W3C 指導方針建議，您應該使用。

執行應用程式中，選取**學生**索引標籤，輸入搜尋字串，然後按一下 [搜尋] 以確認篩選可以運作。

![學生與篩選的索引頁](sort-filter-page/_static/filtering.png)

請注意 「 URL 」 包含搜尋字串。

```html
http://localhost:5813/Students?SearchString=an
```

如果您加入書籤此頁面，您會得到的篩選的清單，當您使用書籤。 加入`method="get"`至`form`標記是什麼造成要產生的查詢字串。

在這個階段，如果您按一下資料行標題排序連結將會遺失您在中輸入篩選值**搜尋**方塊。 您可以修正，下一節。

## <a name="add-paging-functionality-to-the-students-index-page"></a>將分頁功能加入至學生索引頁

若要加入分頁學生索引頁，您將建立`PaginatedList`使用類別`Skip`和`Take`陳述式來篩選資料，而不是永遠擷取所有資料列之資料表的伺服器上。 然後您要進行其他變更的`Index`方法並加入分頁按鈕`Index`檢視。 下圖顯示 [分頁] 按鈕。

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

在專案資料夾中，建立`PaginatedList.cs`，然後將範本程式碼取代下列程式碼。

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync`這個程式碼中的方法會採用頁面大小和頁面數目，並套用適當`Skip`和`Take`陳述式，以`IQueryable`。 當`ToListAsync`上呼叫`IQueryable`，它會傳回包含要求的頁面的清單。 屬性`HasPreviousPage`和`HasNextPage`可用來啟用或停用**上一步**和**下一步**分頁按鈕。

A`CreateAsync`方法用來建立而不是建構函式`PaginatedList<T>`物件，因為建構函式無法執行非同步程式碼。

## <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能加入至索引方法

在*StudentsController.cs*，取代`Index`方法取代下列程式碼。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

這個程式碼會加入頁面的數字參數、 目前的排序順序參數和目前的篩選參數的方法簽章。

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

第一次顯示網頁，或如果使用者尚未按一下分頁或排序連結，所有參數將會都是 null。  按一下分頁連結時，如果頁面變數將包含要顯示的頁面數。

`ViewData` CurrentSort 名為的項目會提供與目前的排序次序中，檢視，因為這必須以保持在分頁時相同的排序順序包含在分頁連結。

`ViewData` CurrentFilter 名為的項目提供具有目前篩選條件字串檢視。 此值必須包含在分頁連結，以篩選設定維護期間分頁，而且它必須還原到文字方塊中時的頁面會重新顯示。

如果搜尋字串變更在分頁時，此頁面具有要重設為 1，因為新的篩選器可能會導致不同的資料，以顯示。 在文字方塊中輸入的值，並按下 [提交] 按鈕時，會變更搜尋字串。 在此情況下，`searchString`參數不是 null。

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

在結尾`Index`方法，`PaginatedList.CreateAsync`方法將學生查詢轉換成單一頁面的學生支援分頁的集合型別。 學生的單一頁面再傳遞到檢視中。

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync`方法會採用頁面數目。 兩個問號表示 null 聯合運算子。 Null 聯合運算子定義預設值為 null 的型別。運算式`(page ?? 1)`方法傳回的值`page`它具有的值，或傳回 1，如果`page`為 null。

## <a name="add-paging-links-to-the-student-index-view"></a>將 Student 索引檢視中的分頁連結

在*Views/Students/Index.cshtml*，以下列程式碼取代現有的程式碼。 所做的變更會反白顯示。

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model`在頁面頂端的陳述式指定檢視現在取得`PaginatedList<T>`物件而非`List<T>`物件。

資料行頁首連結使用查詢字串，將目前的搜尋字串傳遞至控制器，以便使用者可以排序內篩選結果：

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

分頁按鈕會顯示標記協助程式：

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

執行應用程式，並移至學生頁面。

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

按一下分頁中的連結可確定分頁運作的不同排序順序。 然後輸入搜尋字串，然後再試一次以確認，分頁也會搭配正確排序和篩選的分頁。

## <a name="create-an-about-page-that-shows-student-statistics"></a>建立學生統計資料會顯示關於頁面

Contoso 大學網站**有關** 頁面上，您要顯示多少學生已註冊的每個註冊日期。 這需要在群組的群組和簡單計算。 若要這麼做，您會執行下列作業：

* 建立您要傳遞至檢視資料的檢視模型類別。

* 修改主控制器中的關於方法。

* 修改 [關於] 檢視。

### <a name="create-the-view-model"></a>建立檢視模型

建立*SchoolViewModels*資料夾中的*模型*資料夾。

在新的資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>修改主控制器

在*HomeController.cs*，加入下列 using 陳述式在檔案頂端：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

將資料庫內容的類別變數加入之後左大括號類別，並從 ASP.NET Core DI 取得內容的執行個體：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

以下列程式碼取代 `About` 方法：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ 陳述式註冊日期分組的學生實體、 計算每個群組中的實體數目，並將結果儲存在集合中`EnrollmentDateGroup`檢視模型物件。
> [!NOTE] 
> 在 Entity Framework Core 1.0 版中，整個結果集傳回至用戶端，且群組進行用戶端上。 在某些情況下，這可以建立效能問題。 請務必測試效能與實際磁碟區的資料，並如有必要使用原始的 SQL 執行伺服器上的群組。 如需如何使用原始的 SQL，請參閱[這一系列中的最後一個教學課程](advanced.md)。

### <a name="modify-the-about-view"></a>修改檢視

中的程式碼取代*Views/Home/About.cshtml*以下列程式碼檔案：

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

執行應用程式，並移至 [關於] 頁面。 針對每個註冊日期的學生人數會顯示在資料表中。

![有關頁面](sort-filter-page/_static/about.png)

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何執行排序、 篩選、 分頁、 與群組。 在下一個教學課程中，您將學習如何使用移轉作業來處理資料模型變更。

>[!div class="step-by-step"]
[上一頁](crud.md)
[下一頁](migrations.md)  
